# Python进程总览

多进程就是同时进行多项任务，如果把程序比喻成一个手机，那每个app都可以看出是一个进程，你可以同时利用其中一个进程听歌，同时利用另一个进程上网。

### 1、fork\(\):创建子进程、getpid\(\)、getppid\(\)

> 该方法只能在unix/Linux/Mac上运行，windows不可以运行。
>
> 程序执行到fork\(\)时，操作系统会创建一个新进程（子进程），并把父进程的所有信息赋值到子进程中。
>
> 这个方法很特殊，会有两次返回，分别在子进程和父进程返回一次，子进程永远返回0，父进程返回进程的id.
>
> getpid\(\):返回当前进程的id
>
> getppid\(\):返回当前进程父进程的id。

实例：

```
import os
id=os.fork()
index=0
if id<0:
    print("子进程创建失败了")
elif id==0:
    index+=1
    print("我是子进程（%d），我的父进程是：%d,index=%d"%(os.getpid(),os.getppid(),index))
else:
    index += 1
    print("我是父进程：%d，我的子进程是：%d,index=%d"%(os.getpid(),id,index))
```

结果：

```
我是父进程：9735，我的子进程是：9736,index=1
我是子进程（9736），我的父进程是：9735,index=1
```

从上面实例也可以看出：每个进程的所有数据（包括全局变量）都各持一份，互不影响。

多次fork\(\)可发现：父子进程执行顺序没有规律，完全取决于操作系统的调度算法。

### 2、multiprocessing

由于fork\(\)无法对Windows使用，而python是跨平台的，显然需要一个新的跨平台替代品来代替它，那就是multiprocessing模块。

multiprocessing模块中使用Process类来代表进程。

```
语法：Process([group,target,name,args,kwargs])
group:基本用不到，直接忽略
target：进程实例所调用的对象，一般表示子进程要调用的函数。
args：表示调用对象的参数，一般是函数的参数
kwargs：表示调用对象的关键字参数字典。
name：当前进程实例的别名
```

Process类常用方法：

```
is_alive():判断进程是否还在运行。
join([timeout]):是否等待进程实例执行完毕，或等待多少秒
start():启动进程实例(创建子进程）
run():如果没有给定target参数，对该进程对象调用start()方法时，就会执行对象中的run()方法
terminate():不管任务是否完成，立即终止
```

Process类常用属性：

```
name：当前进程实例别名，默认为Process-N，N为从1开始递增的整数。
pid：当前进程实例的PID
```

实例：

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

### 3、继承Process来创建进程

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

### 4、进程池Pool

当我们需要创建大量的进程时，利用multiprocessing模块提供的Pool来创建进程。

进程初始化时，会指定一个最大进程数量，当有新的请求需要创建进程时，如果此时进程池还没有到达设置的最大进程数，该进程池就会创建新的进程来处理该请求，并把该进程放到进程池中，如果进程池已经达到最大数量，请求就会等待，知道进程池中进程数量减少，才会新建进程来执行请求。

multiprocessing.Pool常用函数解析：

```
apply_async(要调用的方法,参数列表,关键字参数列表)：使用非阻塞方式调用指定方法，并行执行（同时执行），
apply(要调用的方法,参数列表,关键字参数列表)：使用阻塞方式调用指定方法，，阻塞方式就是要等上一个进程退出后，下一个j进程才开始运行。
close():关闭进程池，不再接受进的进程请求，但已经接受的进程还是会继续执行。
terminate()：不管程任务是否完成，立即结束。
join():主进程堵塞（就是不执行join下面的语句），直到子进程结束，注意，该方法必须在close或terminate之后使用。
```



实例：

```
from multiprocessing import Pool
import os
import time
import random#用来生成随机数

def test1(name):
    print("%s运行中，pid=%d，父进程：%d"%(name,os.getpid(),os.getppid()))
    t_start=time.time()
    #random.random()会生成一个0——1的浮点数
    time.sleep(random.random()*3)
    t_end=time.time()
    print("执行时间：%0.2f秒"%(name,t_end-t_start))

pool=Pool(5)#设置线程池中最大线程数量为5
for xx in range(0,7):
    #下面一句是线程池调用方
    pool.apply_async(test1,("mark"+str(id),))
print("--start--")
pool.close()#关闭线程池，关闭后不再接受进的请求
pool.join()#等待进程池所有进程都执行完毕后，开始执行下面语句
print("--end--")
```

结果：

```
--start--
mark<built-in function id>运行中，pid=24298，父进程：24297
mark<built-in function id>运行中，pid=24299，父进程：24297
mark<built-in function id>运行中，pid=24300，父进程：24297
mark<built-in function id>运行中，pid=24301，父进程：24297
mark<built-in function id>运行中，pid=24302，父进程：24297
mark<built-in function id>运行中，pid=24302，父进程：24297
mark<built-in function id>运行中，pid=24298，父进程：24297
--end--
```



