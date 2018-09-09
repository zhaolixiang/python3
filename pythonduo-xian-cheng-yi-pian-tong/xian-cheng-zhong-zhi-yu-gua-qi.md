> 线程没有任何方法可用于强制终止或挂起。这是设计上的原因，因为编写线程程序本身十分复杂。例如：如果某个线程已经获取了锁，在它能够释放锁之前强制终止或挂起它，将导致整个应用程序出现死锁。此外，终止时一般不能简单的【释放所有的锁】，因为复杂的线程同步经常涉及锁定和清楚锁定操作，而这些操作在执行时的次序要十分精确。
>
> 如果要为终止或挂起提供服务，需要自己构建这些功能。一般的做法是在循环中运行线程，这个循环的作用是定期检查线程的状态以决定它是否应该终止。例如：

```
from threading import  Thread,Lock
class StoppableThread(Thread):
    def __init__(self):
        Thread.__init__(self)
        self._terminate=False
        self._suspend_lock=Lock()
        
    def terminate(self):
        self._terminate=True
    
    def suspend(self):
        self._suspend_lock.acquire()
        
    def resume(self):
        self._suspend_lock.release()
    
    def run(self):
        while True:
            if self._terminate:
                break
            self._suspend_lock.acquire()
            self._suspend_lock.release()
            ...
```

要记住，要让这种方法可靠的工作，线程应该千万小心不要执行任何类型的阻塞I/O操作。例如，如果线程阻塞等待数据到达，那么它会直到该操作被唤醒时才会终止。因此，你需要在实际中使用超时、非阻塞I/O和其它高级功能，从而确保终止检查执行的频率足够。

