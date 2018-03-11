---
title: Python-30-multiprocessing
date: 2018-03-10 12:14:43
categories: Python
tags:
- Python
- multiprocessing
- pool
---



进程中可以包含多个线程，多个任务并行处理时可以多进程，也可以多线程来实现。

# Python中实现多进程

Unix/Linux中通过os.fork()实现创建子进程。

由于windows系统没有fork调用，故跨平台应用时，可使用multiprocessing模块。

```python
from multiprocessing import Process
import os

# Child process
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))
    
if __name__ == '__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process start.')
    p.start()
    p.join()
    print('Child process end.')
```

通过向Process函数内传入执行的函数和参数，并用start方法开始执行，用join方法等待子进程结束再往下运行。

执行结果为，

```python
Parent process 8808.
Child process will start.
Run child process test (7680)...
Child process end.
```

# 进程池pool

使用Pool启动大量进程，

```python
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All subprocesses done.')
```

join方法等待所有子进程执行完毕，在join之前必须加入close方法，使之后不能继续添加新的process。

执行结果为，

```python
Parent process 4916.
Waiting for all subprocesses done...
Run task 0 (6708)...
Run task 1 (800)...
Run task 2 (8420)...
Run task 3 (9724)...
Task 2 runs 1.16 seconds.
Run task 4 (8420)...
Task 0 runs 1.88 seconds.
Task 1 runs 1.93 seconds.
Task 3 runs 1.92 seconds.
Task 4 runs 0.79 seconds.
All subprocesses done.
```

可知，Pool(4)新建了4个进程，其中task 0-3是立刻执行。而task 4则要等待0-3中某一个执行完毕才能开始，因为pool中只开了4个槽，无法同时容纳5个进程。

不指定的话，pool默认为CPU的内核数目。

# 参考

廖雪峰Python3教程