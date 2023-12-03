---
title: asst2
date: 2023-11-29
lastmod: 2023-12-04
author: ['Ysyy']
categories: ['']
tags: ['cmu-15418&cs-618']
description: 
weight: None
draft: False
comments: True
showToc: True
TocOpen: True
hidemeta: False
disableShare: False
showbreadcrumbs: True
---
## C++ Sync

thread的使用

```C++
#include <thread>
#include <stdio.h>

void my_func(int thread_id, int num_threads) {
 printf("Hello from spawned thread %d of %d\n", thread_id, num_threads);
}

int main(int argc, char** argv) {

  std::thread t0 = std::thread(my_func, 0, 2);
  std::thread t1 = std::thread(my_func, 1, 2);

  printf("The main thread is running concurrently with spawned threads.\n");

  t0.join();
  t1.join();

  printf("Spawned threads have terminated at this point.\n");

  return 0;
}
```

mutex

```C++
#include <chrono>
#include <iostream>
#include <map>
#include <mutex>
#include <string>
#include <thread>
 
std::map<std::string, std::string> g_pages;
std::mutex g_pages_mutex;
 
void save_page(const std::string& url)
{
    // simulate a long page fetch
    std::this_thread::sleep_for(std::chrono::seconds(2));
    std::string result = "fake content";
 
    std::lock_guard<std::mutex> guard(g_pages_mutex);
    g_pages[url] = result;
}
 
int main() 
{
    std::thread t1(save_page, "http://foo");
    std::thread t2(save_page, "http://bar");
    t1.join();
    t2.join();
 
    // safe to access g_pages without lock now, as the threads are joined
    for (const auto& pair : g_pages)
        std::cout << pair.first << " => " << pair.second << '\n';
}
```

Output

```shell
http://bar => fake content
http://foo => fake content
```

## part_a

### step 1 实现TaskSystemParallelSpawn

```c++
void TaskSystemParallelSpawn::run(IRunnable *runnable, int num_total_tasks)
{

    //
    // TODO: CS149 students will modify the implementation of this
    // method in Part A.  The implementation provided below runs all
    // tasks sequentially on the calling thread.
    //

    std::atomic<int> taskId(0);
    int num_threads = this->num_threads;

    std::thread threads[num_threads];
    // 交叉分配任务

    for (int i = 0; i < num_threads; i++)
    {
        threads[i] = std::thread([&, i]()
                                 {
                int task_id = taskId.fetch_add(1);
                while (task_id < num_total_tasks)
                {
                    runnable->runTask(task_id, num_total_tasks);
                    task_id = taskId.fetch_add(1);
                } });
    }
    for (int i = 0; i < num_threads; i++)
    {
        threads[i].join();
    }

    // printf("done\n");
}
```

Q:How will you assign tasks to your worker threads? Should you consider static or dynamic assignment of tasks to threads?
A:交叉分配任务，动态分配任务

Q:How will you ensure that all tasks are executed exactly once?
A:使用原子变量taskId

### step 2 实现  TaskSystemParallelThreadPoolSpinning

step1 的overhead主要是创建线程的开销(尤其是计算量低的任务上)，因此使用线程池可以减少开销

要求: 在TestSystem 创建时,或者在run时创建线程池

Q1: 作为一个开始的实现，我们建议您将worker threads设计为连续循环，始终检查它们是否有更多的工作要执行。(进入 while 循环直到条件为真的线程通常称为“spinning”)
那么worker thread 如何确定有work要执行呢？

```c++
TaskSystemParallelThreadPoolSpinning::TaskSystemParallelThreadPoolSpinning(int num_threads) : ITaskSystem(num_threads)
{
    //
    // TODO: CS149 student implementations may decide to perform setup
    // operations (such as thread pool construction) here.
    // Implementations are free to add new class member variables
    // (requiring changes to tasksys.h).
    //
    exit_flag_ = false;
    for (int i = 0; i < num_threads; i++)
    {
        threads.emplace_back(&TaskSystemParallelThreadPoolSpinning::func, this);
    }
}

TaskSystemParallelThreadPoolSpinning::~TaskSystemParallelThreadPoolSpinning()
{
    exit_flag_ = true;
    for (auto &thread : threads)
    {
        thread.join();
    }
}

void TaskSystemParallelThreadPoolSpinning::run(IRunnable *runnable, int num_total_tasks)
{

    //
    // TODO: CS149 students will modify the implementation of this
    // method in Part A.  The implementation provided below runs all
    // tasks sequentially on the calling thread.
    //
    // printf("run\n");
    runnable_ = runnable;
    num_tasks_ = num_total_tasks;
    num_tasks_done_ = num_total_tasks;
    for (int i = 0; i < num_total_tasks; i++)
    {
        tasks_mutex_.lock();
        tasks_.push(i);
        tasks_mutex_.unlock();
    }
    while (num_tasks_done_ < num_total_tasks)
    {
        std::this_thread::yield();
    };
    // Q:为什么要使用yield
    // A:因为如果不使用yield，那么线程会一直占用CPU，导致其他线程无法运行
    // Q:那我直接死循环呢
    // A:死循环会导致CPU占用率100%，导致其他线程无法运行
}
```

Q2:确保 run ()实现所需的同步行为是非常重要的。如何更改 run ()的实现以确定批量任务启动中的所有任务都已完成？
A:使用原子变量num_tasks_done_，每个任务完成时，num_tasks_done_加一，当num_tasks_done_等于num_total_tasks时，所有任务完成

### step 3 实现 TaskSystemParallelThreadPoolSleeping

Step2的缺点：
当线程“spin”等待某些操作时，它们会利用 CPU 核心的执行资源。

- 例如，工作线程可能会循环等待新任务到达。
- 另一个例子是，主线程可能会循环等待辅助线程完成所有任务，这样它就可以从 run ()调用返回。

这可能会影响性能，因为即使这些线程没有做有用的工作，也会使用 CPU 资源来运行这些线程。

在任务的这一部分中，我们希望您通过让线程处于休眠状态来提高任务系统的效率，直到它们所等待的条件得到满足。

您的实现可以选择使用条件变量来实现此行为。条件变量是一个同步原语，它允许线程在等待条件存在时休眠(不占用 CPU 处理资源)。其他线程向等待唤醒的线程发出“信号”，以查看它们所等待的条件是否已经满足。例如，如果没有工作要做，您的工作线程可能会处于休眠状态(这样它们就不会从尝试执行有用工作的线程那里占用 CPU 资源)。另一个例子是，调用 run ()的主应用程序线程可能希望在等待批量任务启动中的所有任务由工作线程完成时休眠。(否则，一个旋转的主线程将从工作线程那里夺走 CPU 资源!)有关 C + + 中条件变量的更多信息，请参见我们的 C + + 同步教程。

您在这部分作业中的实现可能需要考虑棘手的race conditions 。您需要考虑许多可能的线程行为交错

您可能需要考虑编写额外的测试用例来测试您的系统。赋值入门代码包括评分脚本用于评分代码性能的工作负载，但是我们也将使用一组更广泛的工作负载来测试您的实现的正确性，而我们在入门代码中并没有提供这些工作负载！

The assignment starter code includes the workloads that the grading script will use to grade the performance of your code, but we will also test the correctness of your implementation using a wider set of workloads that we are not providing in the starter code!