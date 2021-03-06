# 2、multiprocessing

由于fork\(\)无法对Windows使用，而python是跨平台的，显然需要一个新的跨平台替代品来代替它，那就是multiprocessing模块。

multiprocessing模块中使用Process类来代表进程。

```
语法：Process([group,target,name,args,kwargs])
group:至今还未使用，值始终为None
target：进程实例所调用的对象，一般表示子进程要调用的函数。
args：表示调用对象的参数，一般是函数的参数
kwargs：表示调用对象的关键字参数字典。
name：当前进程实例的别名
```

Process类常用方法：

```
p.is_alive():判断进程是否还在运行。如果还在运行，返回true，否则返回false
p.join([timeout]):等待进程实例执行完毕，或等待多少秒
p.run():默认会调用target指定的对象，如果没有给定target参数，对该进程对象调用start()方法时，就会执行对象中的run()方法
p.start():启动进程实例(创建子进程）,病运行子进城的run方法
p.terminate():不管任务是否完成，立即终止,同时不会进行任何的清理工作，如果进程p创建了它自己的子进程，这些进程就会
变成僵尸进程，使用时特别注意，如果p保存了一个锁或者参与了进程间通信，那么使用该方法终止它可能会导致死锁或者I/O损坏。
```

Process类常用属性：

```
p.daemon：布尔值，指示进程是否是后台进程。当创建它的进程终止时，后台进程会自动终止。并且，后台进程无法创建自己的新进城。
注意：p.daemon的值必须在p.start方法调用前设置。
p.exitcode：进程的整数退出指令。如果进程仍然在运行，它的值为None，如果值为负数：—N，就表示进程由信号N所终止。
p.name：当前进程实例别名，默认为Process-N，N为从1开始递增的整数。
p.pid：当前进程实例的PID
```

**实例1：理解单独创建进程的相关函数**

```
 #该实例是用来理解单独创建进程的实例
from multiprocessing import Process
import os,time

#将要在子进程中运行的方法
def test(name,interval):
    for i in range(interval):
        print("子进程运行中，name=%s，pid=%d，父进程：%d"%(name,os.getpid(),os.getppid()))
        time.sleep(interval)

if __name__=="__main__":
    print("父进程%d"%os.getpid())
    #创建进程实例，第一个参数传要在子线程执行的函数，第二个参数传函数需要的参数
    p=Process(target=test,args=('mark',2))
    print("子进程要执行了")
    #启动进程
    p.start()
    p.join()#等待子进程运行结束再继续执行下面语句
    print("子进程结束了")
```

结果：

```
父进程17756
子进程要执行了
子进程运行中，name=mark，pid=17758，父进程：17756
子进程运行中，name=mark，pid=17758，父进程：17756
子进程结束了
```

**实例2：两个进程同时运行**

```
from multiprocessing import Process
import os
import time

def test1(interval):
    print("test1子进程运行中，pid=%d，父进程：%d"%(os.getpid(),os.getppid()))
    t_start=time.time()
    time.sleep(interval)
    t_end=time.time()
    print("test1执行时间：%0.2f秒"%(t_end-t_start))


def test2(interval):
    print("test2子进程运行中，pid=%d，父进程：%d"%(os.getpid(),os.getppid()))
    t_start=time.time()
    time.sleep(interval)
    t_end=time.time()
    print("test2执行时间：%0.2f秒"%(t_end-t_start))

if __name__=="__main__":
    print("父进程%d"%os.getpid())
    #创建进程实例，第一个参数传要在子线程执行的函数，第二个参数传函数需要的参数
    p1=Process(target=test1,args=(1,))
    p2=Process(target=test2,name="mark1",args=(2,))
    #启动进程
    p1.start()
    p2.start()

    print("p2是否在运行：",p2.is_alive())

    p2.join()#等待子进程运行结束再继续执行下面语句
    print("p2是否在运行：", p2.is_alive())
```

结果：

```
父进程15080
p2是否在运行： True
test1子进程运行中，pid=15081，父进程：15080
test2子进程运行中，pid=15082，父进程：15080
test1执行时间：1.00秒
test2执行时间：2.00秒
p2是否在运行： False
```



