[参考](https://www.cnblogs.com/kalicener/p/16824312.html)

[任务](https://github.com/stanford-cs149/asst1)

## [Program 1: Parallel Fractal Generation Using Threads (20 points)](https://github.com/stanford-cs149/asst1#program-1-parallel-fractal-generation-using-threads-20-points)

任务描述:
用多线程画mandelbrot fractal.

代码中给出了串行的实现, 你需要实现多线程的版本.

多线程版本中只需要修改`workerThreadStart`函数.
不需要手动创建线程, 也不需要手动join线程.
直接调用mandelbrotThread().

结果:

|线程数|加速比|
|---|---|
|2|1.97|
|3|1.63|
|4|2.43|
|5|2.37|
|6|2.55|
|7|2.895|
|8|143.013|
