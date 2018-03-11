---
title: Python-31-MPapply
date: 2018-03-10 12:54:42
categories: Python
tags:
- multiprocessing
- apply
- apply_async

---

Python synchronous & asynchronous

Python的多进程实现中，会用到Pool开启一个进程池。当要启动进程池中的进程时，有两种方法：一是apply()，另一个是apply_async()。最直观的理解就是，apply是阻塞的，而apply_sync是非阻塞的。

# apply

阻塞，意味着子进程按照顺序依次执行，前一个进程未结束时，后一个进程并不会执行。

父进程会等待所有子进程执行完毕再执行。

```python
# 
from multiprocessing import Pool
import os, time, random

def task(name):
    print('Child process %s(%s) start' % (name, os.getpid()))
    child_start = time.time()
    time.sleep(random.random() * 3)
    child_end = time.time()
    print('Task %s runs %0.2f seconds' % (name, child_end - child_start))
    
if __name__ == '__main__':
    print('Parent process %s start' % os.getpid())
    parent_start = time.time()
    p = Pool(4)
    for i in range(5):
        p.apply(task, args=(i,))
    print('Wait for all task done')
    p.close()
    parent_end = time.time()
    print('All tasks done')
    print('main task runs %0.2f seconds' % (parent_end - parent_start))
```

执行结果为，

```
Parent process 6248 start
Child process 0(7432) start
Task 0 runs 1.50 seconds
Child process 1(6476) start
Task 1 runs 2.38 seconds
Child process 2(7064) start
Task 2 runs 2.14 seconds
Child process 3(3820) start
Task 3 runs 0.75 seconds
Child process 4(7432) start
Task 4 runs 2.97 seconds
Wait for all task done
All tasks done
main task runs 10.12 seconds
```

这等价于按顺序新建5个进程，再依次执行。

```python
for i in range(5):
    p = multiprocessing.Process(target=task, args=(i,))
    p.start()
    p.join()
```

`join()`方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步。

# apply_sync

非阻塞，意味着一定数目的子进程同时开始运行。

按照顺序依次执行，前一个进程未结束时，后一个进程并不会执行。

## 等待子进程结束join()

```python
# 
from multiprocessing import Pool
import os, time, random

def task(name):
    print('Child process %s(%s) start' % (name, os.getpid()))
    child_start = time.time()
    time.sleep(random.random() * 3)
    child_end = time.time()
    print('Task %s runs %0.2f seconds' % (name, child_end - child_start))
    
if __name__ == '__main__':
    print('Parent process %s start' % os.getpid())
    parent_start = time.time()
    p = Pool(4)
    for i in range(5):
        p.apply_async(task, args=(i,))
    print('Wait for all task done')
    p.close()
    # 此处父进程等待Pool中所有子进程结束
    p.join()
    parent_end = time.time()
    print('All tasks done')
    print('main task runs %0.2f seconds' % (parent_end - parent_start))
```

Pool对象调用join函数等待所有子进程执行完毕再继续下面的代码。

得到的输出为，

```
Parent process 4332 start
Wait for all task done
Child process 0(3000) start
Child process 1(1728) start
Child process 2(4568) start
Child process 3(9368) start
Task 3 runs 0.06 seconds
Child process 4(9368) start
Task 1 runs 2.41 seconds
Task 2 runs 2.59 seconds
Task 0 runs 2.80 seconds
Task 4 runs 2.90 seconds
All tasks done
main task runs 3.48 seconds
```

## 不等待子进程结束

如果不需要等待子进程，父进程会很快执行完毕。如下，

```python
# 
from multiprocessing import Pool
import os, time, random

def task(name):
    print('Child process %s(%s) start' % (name, os.getpid()))
    child_start = time.time()
    time.sleep(random.random() * 3)
    child_end = time.time()
    print('Task %s runs %0.2f seconds' % (name, child_end - child_start))
    
if __name__ == '__main__':
    print('Parent process %s start' % os.getpid())
    parent_start = time.time()
    p = Pool(4)
    for i in range(5):
        p.apply_async(task, args=(i,))
    print('Wait for all task done')
    p.close()
    # p.join()
    parent_end = time.time()
    print('All tasks done')
    print('main task runs %0.2f seconds' % (parent_end - parent_start))
```

把join注释掉之后，开启进程池之后，父进程继续向下执行，直到结束都还没来得及切换到子进程。

结果如下，

```
Parent process 7868 start
Wait for all task done
All tasks done
main task runs 0.10 seconds
```

如果让父进程再等待一段时间，比如加上sleep，

```python
    ...
    p.close()
    time.sleep(2)
    parent_end = time.time()
    print('All tasks done')
    ...
```

得到，

```
Parent process 10180 start
Wait for all task done
Child process 0(9860) start
Child process 1(9168) start
Child process 2(7608) start
Child process 3(8148) start
Task 2 runs 0.81 seconds
Child process 4(7608) start
Task 4 runs 0.01 seconds
Task 1 runs 1.49 seconds
All tasks done
main task runs 2.19 seconds
```

可知，Task 3还没来得及执行，父进程就结束了，整个程序也退出了。

# 参考

多进程

https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431927781401bb47ccf187b24c3b955157bb12c5882d000

python多进程apply与apply_async的区别

https://www.jianshu.com/p/0a55507f9d9e?open_source=weibo_search