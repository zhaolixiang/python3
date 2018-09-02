# 3、继承Process来创建进程

实例：

```
from multiprocessing import Process
import os
import time
class MyProcess(Process):
    #重新init方法
    def __init__(self,interval):
        #下面一句是调用父类init方法，这一本尽量不要少，因为父类还有很多事情需要在init方法内处理
        Process.__init__(self)
        self.interval=interval

    #重写run方法
    def run(self):
        print("子进程运行中，pid=%d，父进程：%d" % (os.getpid(), os.getppid()))
        t_start=time.time()
        time.sleep(self.interval)
        t_end=time.time()
        print("子进程运行结束，耗时：%0.2f秒"%(t_end-t_start))

if __name__=="__main__":
    t_start=time.time()
    print("父进程开始执行")
    p=MyProcess(2)
    p.start()
    p.join()
    t_end=time.time()
    print("父进程运行结束，耗时：%0.2f秒" % (t_end - t_start))
```

结果：

```
父进程开始执行
子进程运行中，pid=20728，父进程：20727
子进程运行结束，耗时：2.00秒
父进程运行结束，耗时：2.02秒
```



