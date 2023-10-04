[参考](https://www.cnblogs.com/kalicener/p/16824312.html)

[任务](https://github.com/stanford-cs149/asst1)

## [Program 1: Parallel Fractal Generation Using Threads (20 points)](https://github.com/stanford-cs149/asst1#program-1-parallel-fractal-generation-using-threads-20-points)

任务描述:
用多线程画mandelbrot fractal.

代码中给出了串行的实现, 你需要实现多线程的版本.

多线程版本中只需要修改 `workerThreadStart`函数.
不需要手动创建线程, 也不需要手动join线程.
直接调用mandelbrotThread().

### 1.1 & 1.2, 计算在2,3,4,5,6,7,8,16,32个线程下的加速比

#### 编写并观察

workerThreadStart函数的实现:

```C++
345void workerThreadStart(WorkerArgs *const args)
{

    // TODO FOR CS149 STUDENTS: Implement the body of the worker
    // thread here. Each thread should make a call to mandelbrotSerial()
    // to compute a part of the output image.  For example, in a
    // program that uses two threads, thread 0 could compute the top
    // half of the image and thread 1 could compute the bottom half.

    // printf("Hello world from thread %d\n", args->threadId);
    double startTime = CycleTimer::currentSeconds();
    // 每个线程负责的行数(除不尽的部分由最后一个线程负责)
    int height = args->height / args->numThreads;
    int startRow = args->threadId * height;
    int numRows = height;
    if (args->threadId == args->numThreads - 1)
    {
        // 如果是最后一个线程，那么就要把除不尽的部分也算上
        numRows = height + args->height % args->numThreads;
    }
    printf("Thread %d startRow: %d, numRows: %d\n", args->threadId, startRow, numRows);
    mandelbrotSerial(args->x0, args->y0, args->x1, args->y1,
                     args->width, args->height,
                     startRow, numRows,
                     args->maxIterations, args->output);
    double endTime = CycleTimer::currentSeconds();
    printf("Thread %d time: %.3f ms\n", args->threadId, (endTime - startTime) * 1000);
}
```

结果:

| 线程数 | 加速比 |
| ------ | ------ |
| 2      | 1.97   |
| 3      | 1.63   |
| 4      | 2.31   |
| 5      | 2.37   |
| 6      | 3.08   |
| 7      | 3.15   |
| 8      | 3.74   |
| 16     | 5.14   |

可以观察到，加速比和线程数并不是线性相关.

#### 猜测原因

猜测可能的原因有:

- 线程通信的开销
- 每个线程分配的任务不均匀

### 1.3 查看每个线程的执行时间,验证猜想

当线程数为4时, 每个线程的执行时间如下:
Thread 0 time: 63.974 ms
Thread 3 time: 65.563 ms
Thread 2 time: 259.972 ms
Thread 1 time: 260.669 ms

当线程数为8时, 每个线程的执行时间如下:
Thread 0 time: 13.702 ms
Thread 7 time: 16.831 ms
Thread 1 time: 57.324 ms
Thread 6 time: 61.069 ms
Thread 5 time: 113.431 ms
Thread 2 time: 115.753 ms
Thread 4 time: 164.736 ms
Thread 3 time: 166.306 ms

可以看到,中间线程分配的任务更多,执行时间更长.
因此在增加线程数时,加速比并不是线性增加的.

### 1.4

任务描述:

- 解决上面的问题,使得加速比更接近线性.
  - 如: 8线程时的加速比需要在7~8之间.
- 解决方法需要具有适用性, 适用所有的线程数.

tips:
有一个非常简单的静态赋值可以实现这个目标，并且线程之间不需要通信/同步.

#### 解决方案

思路:
根据代码可知, 每行的计算是独立的, 因此可以将每行分配给不同的线程.
但由上面的实验可知,中间行的计算量比较大.

因此我们不应该直接平均切分行, 而是以线程数量为步长,线程交叉依次分配行.
即 第i个线程分配k*n+i行.

```C++
void workerThreadStart(WorkerArgs *const args)
{

    // TODO FOR CS149 STUDENTS: Implement the body of the worker
    // thread here. Each thread should make a call to mandelbrotSerial()
    // to compute a part of the output image.  For example, in a
    // program that uses two threads, thread 0 could compute the top
    // half of the image and thread 1 could compute the bottom half.

    // printf("Hello world from thread %d\n", args->threadId);
    double startTime = CycleTimer::currentSeconds();

    /*
    方案1
    // 每个线程负责的行数(除不尽的部分由最后一个线程负责)
    int baseHeight = args->height / args->numThreads;
    int startRow = args->threadId * baseHeight;
    int numRows = baseHeight;

    int yu = args->height % args->numThreads;
    // 均匀分配剩余行
    if (args->threadId < yu)
    {
        numRows++;
    }
    startRow += std::min(args->threadId, yu);
    printf("Thread %d startRow: %d, numRows: %d\n", args->threadId, startRow, numRows);
    mandelbrotSerial(args->x0, args->y0, args->x1, args->y1,
                     args->width, args->height,
                     startRow, numRows,
                     args->maxIterations, args->output);

    */

    // 方案2, 依次分配行
    int height = args->height;
    for (int i = args->threadId; i < height; i += args->numThreads)
    {
        mandelbrotSerial(args->x0, args->y0, args->x1, args->y1,
                         args->width, args->height,
                         i, 1,
                         args->maxIterations, args->output);
    }

    double endTime = CycleTimer::currentSeconds();
    printf("Thread %d time: %.3f ms\n", args->threadId, (endTime - startTime) * 1000);
}
```

输出结果:

Thread 3 time: 88.842 ms
Thread 1 time: 89.680 ms
Thread 0 time: 89.717 ms
Thread 7 time: 90.280 ms
Thread 5 time: 90.715 ms
Thread 6 time: 90.743 ms
Thread 2 time: 91.049 ms
Thread 4 time: 92.982 ms
[mandelbrot thread]:            [93.318] ms
Wrote image file mandelbrot-thread.ppm
                                (7.10x speedup from 8 threads)

上面的解决方案使得每个线程的执行时间基本相同,因此加速比接近线性.
在8线程时,加速比为7.1.

### 1.5 16线程和8线程的加速比

现在16线程是否明显优于8线程? 给出是或否的原因.
  (6.45x speedup from 16 threads)
16线程并没有明显由于8线程,反而还更慢.
原因:

- 电脑本身是4核, 超线程后是8线程.
- 16线程时线程切换反而导致开销增加.

### 总结

pro1的目的是为了认识到并行计算的overhead, 以及多线程在计算上也应该是依次交替分配的. 不能简单的平均分配.

而下一章的pro2就是通过向量化+simd指令来减少overhead.

## program-2-vectorizing-code-using-simd-intrinsics
