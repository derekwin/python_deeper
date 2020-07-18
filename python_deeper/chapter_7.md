### 线程，进程，协程

#### 线程

> GIL锁（目前python还没有移除GIL锁）~2020.4~

线程是操作系统能够进行运算调度的最小单位

python 中的并发编程受限于GIL锁，只能跑在单核处理器上，并且GIL锁内多线程运行时，要频繁的加锁解锁切换线程，并发性能很差。

Python 的线程更适用于处理 I/O 和其他需要并发执行的阻塞操作（比如等待 I/O、等待从数据库获取数据等等），而不是需要多处理器并行的计算密集型任务



##### 多线程编程

简单的线程启动

```python
from threading import Thread
import time
def func(n):
    while n>10:
        print("last {} s".format(n))
        n-=1
        time.sleep(2)
        
t= Thread(target=func,args=(20,))
t.start()
```

```python
# 线程存活判断
t.is_alive()
# 设为守护线程
t = Thread(target=func,args=(20,),daemon=True)
# 使用setDaemon(True)方法，设置子线程为守护线程时，主线程一旦执行结束，则全部线程全部被终止执行，可能出现的情况就是，子线程的任务还没有完全执行结束，就被迫停止
for t in thread_list:
    t.setDaemon(True)
    t.start()
```



用类的方式写线程任务，实现可以中断

```python
class task:
    def __init__(self,value):
        self._running=True
        self.value=value
        
    def terminate(self):
        self._running=False
        
    def run(self,n):
        while self._running==True:			# 轮询点
            print("running")
            time.sleep(2)
            
task1=task(2)
t=Thread(target=task1.run,args=(2,))
t.start()
task1.terminate()
t.join()
# join所完成的工作就是线程同步，即主线程任务结束之后，进入阻塞状态，一直等待其他的子线程执行结束之后，主线程在终止，
for t in thread_list:
    t.join()
```



##### 线程池

【concurrent.futures】

```python
from concurrent.futures import ThreadPoolExecutor
def task(a,b):
    pass

pool=ThreadPoolExecutor(128)
pool.submint(task,a,b)

## 队列实现
from queue import Queue
def task(q):
    a,b=q.get()
    pass

q=Queue()
k=128
for i in range(k):
    t=Thread(target=task,args=(q,))
    t.daemon=True
    t.start
q.put(a,b)    

```



##### 线程间通信

【队列Queue】最优选择

同 进程



##### 线程锁

```python
import threading
# 一种比较好的写法，使用with
class test:
    def __init__(self):
        self.lock=threading.lock()
    def task1(self):
        with self.lock:
            pass
    def task2(self):
        with self.lock:
            pass
```

##### 死锁避免

思路：构造一个acquire函数，给每个锁一个唯一id，将多个锁的加锁顺序固定化

```python
import threading
from contextlib import contextmanager

_local = threading.local()
@contextmanager
def acquire(*locks):
    # Sort locks by object identifier
    locks = sorted(locks, key=lambda x: id(x))
    # Make sure lock order of previously acquired locks is not violated
    acquired = getattr(_local,'acquired',[])
    if acquired and max(id(lock) for lock in acquired) >= id(locks[0]):
        raise RuntimeError('Lock Order Violation')
    # Acquire all of the locks
    acquired.extend(locks)
    _local.acquired = acquired
    try:
        for lock in locks:
            lock.acquire()
        yield
    finally:
        # Release locks in reverse order of acquisition
        for lock in reversed(locks):
            lock.release()
        del acquired[-len(locks):]

```





#### 进程

是计算机中的程序关于某数据集合上的一次运行活动，是==系统进行资源分配和调度的基本单位==，是操作系统结构的基础。

##### 创建一个子进程

```python
from multiprocessing import Process

def run():
    print("running")
 
if __name__=="__main__":
    p=Process(target=run(),args=('a',))
    p.start()
    p.join()								# 同线程
```

##### 进程池

```python
from multiprocessing import Pool
def task(num):
    print("task{}".format(num))
if __name__=="__main__":
    p=Pool(5)
    for i in range(5):
        p.apply_async(task,(what,))
    p.close()
    p.join()
    
# 对Pool对象调用join()方法会等待所有子进程执行完毕，调用join()之前必须先调用close()，调用close()之后就不能继续添加新的Process了。
```

##### 添加外部子进程

【subprocess】

https://www.jianshu.com/p/2eb33b491024



##### 进程间通信

数据通信  Queue & Pipes

```python
from multiprocessing import Queue,Process
import time

def write(q):
    print("w")
    for i in range(5):
        q.put(i)
        time.sleep(2)
def read(q):
    print("r")
    while True:
        value=q.get(True)
        print(value)

if __name__=="__main__":
    q=Queue()
    pw=Process(target=write,args=(q,))
    pr=Process(target=read,args=(q,))

    pw.start()
    pr.start()

    pw.join()
    pr.terminate()			# read进程是死循环，人为终断
```



分布式分发任务

利用pickle，更好的选择是json 进行数据的序列化和反序列化



> 6种进程间的通信方式
>
> msg_queue (消息队列)
> pipeline for single duplex (单工管道)
> pipeline for half duplex (半双工管道)
> name pipeline (命名管道)
> share memory (共享内存)
> semaphore (信号量)





#### 协程

> 协程是为==非抢占式==多任务产生子程序的计算机程序组件，协程允许不同入口点在不同位置暂停或者开始执行任务

- **协程和线程的异同点**

    相似之处：多协程和多线程一样，只会交叉喘息执行

    不同处：线程间频繁切换，加锁解锁。协程间通过yield暂停生成器，将程序执行交给其他子程序，流式执行

- **协程**

    - 协程是在单线程里面实现任务的切换

    - 协程利用同步的方式实现异步

    - 协程不需要锁，提高了并发线程

      ​    

> [转到生成器](#生成器)
>
> 在函数暂停后，向生成器发送内容，使用```生成器.send(None)```的方式

```python
# .send()的案例
def test(n):
	x=0
	while x<n:
		y= yield x
		if y is None:
			y=1
		x+=y
# python3.8 这块是会自动抛出StopIteration异常的
	
if __name__ == "__main__":
    gen=test(5)
    print(next(gen))
    print(gen.send(2))
```













### 事件型异步 多进程分布式实现

✔ Twisted 事件型网络框架

✔ Socket 编程   -> Tcp/Udp驱动的高并发模型



asyncio