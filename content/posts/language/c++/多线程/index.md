---
title: 多线程
date: 2023-12-08
lastmod: 2024-03-02
author: ['Ysyy']
categories: ['']
tags: ['c++']
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
## 锁

### unique_lock 和 lock_guard

unique_lock 和 lock_guard 都是 RAII 的封装，都是用来管理 mutex 的，但是 unique_lock 比 lock_guard 更加灵活，可以随时 unlock 和 lock，而 lock_guard 只能在构造的时候 lock，在析构的时候 unlock。

unique_lock:

    ```cpp
    std::mutex mtx;
    std::unique_lock<std::mutex> lck(mtx);
    lck.unlock();
    lck.lock();
    ```

lock_guard:

    ```cpp
    std::mutex mtx;
    std::lock_guard<std::mutex> lck(mtx);
    ```

#### 和 condition_variable 使用时的区别

unique_lock 和 lock_guard 都可以和 condition_variable 一起使用，但是 unique_lock 更加灵活，可以随时 unlock 和 lock，而 lock_guard 只能在构造的时候 lock，在析构的时候 unlock。

unique_lock:

    ```cpp
    std::mutex mtx;
    std::condition_variable cv;
    std::unique_lock<std::mutex> lck(mtx);
    cv.wait(lck);
    /*
    这部分仍然被锁住
    */
    lck.unlock();
    lck.lock();
    ```
lock_guard:

    ```cpp
    std::mutex mtx;
    std::condition_variable cv;
    std::lock_guard<std::mutex> lck(mtx);
    cv.wait(lck);

    /*
    这部分已经被解锁
    */
    ```