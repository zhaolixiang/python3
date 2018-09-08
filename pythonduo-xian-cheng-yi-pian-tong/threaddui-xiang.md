# Thread对象

Thread类用于表示单独的控制线程。

### 语法：

```
t=Thread(group=None,target=None,name=None,args=(),kwargs={})
创建一个新的Thread实例：t

group：为以后扩张保留的，默认为None

target：一个可调用对象，线程启动时，run()方法将调用此对象

name：线程名称，默认创建一个“Thread-N”格式的唯一名称。

args:传递给target函数的参数元祖

kwargs:传递给target的关机字参数的字典。
```

### 常用属性于方法

```
t.start():通过在一个单独的控制线程中调用run(),启动线程，此方法只能被调用一次。

t.run()：线程启动时将调用此方法。默认情况下，他会调用目标函数target。还可以在Thread的子类中重新定义此方法。

t.join([timeout])：阻塞线程，等待直到线程终止或者出现超时为止。timeout是以秒为单位的超时时间。
线程启动之前不能调用此方法，否则会报错。

t.is_alive：如果线程是活动的，返回True，否则返回False，从start()返回的那一刻开始，线程就是活动的，
直到run()方法终止为止。

t.name：线程名称，这个字符串用于唯一标识，可以根据需要将它更改为更有意义的值，

t.ident：整数线程标识符，如果线程尚未启动，它的值为None。

t.daemon：如果线程是后台线程，该值为True，否则未False。当不存在任何任何活动的非后台进程时，整个程序会退出。
```

### 实例1：利用Thread对象，简单创建一个线程，并启动一个函数

##### 代码：

```
#利用Thread对象，简单创建一个线程，并启动一个函数
from threading import Thread
import time
def mark(interval):
    print("循环等待时间时间%d,等待前时间：%s"%(interval,time.ctime()))
    time.sleep(interval)
    print("等待后的时间：%s"%time.ctime())

if __name__=="__main__":
    t=Thread(target=mark,args=(3,))
    t.daemon=False#设置为非后台线程，不然会看不到自线程打印效果主线程就直接关闭了
    #下面一句会报错，必须在start()方法之后调用
    #t.join(3)
    t.start()
    #下面语句也会报错，因为start只能调用一次
    #t.start()
    print("end")
```

##### 结果：

![](/assets/4、利用Thread对象，简单创建一个线程，并启动一个函数.gif)

### 实例2：通过继承Thread，实现线程类

代码：

```
#通过继承Thread，实现线程类
from threading import Thread
import time

class MyThread(Thread):
    def __init__(self,interval):
        Thread.__init__(self)
        self.daemon=False
        self.interval=interval
    def run(self):
        print("循环等待时间时间%d,等待前时间：%s" % (self.interval, time.ctime()))
        time.sleep(self.interval)
        print("等待后的时间：%s" % time.ctime())

if __name__=="__main__":
    t=MyThread(3)
    t.start()
    #为了方便查看打印效果，加了一秒延迟
    time.sleep(1)
    print("end")
```

结果：

![](/assets/5、通过继承Thread，实现线程类.gif)



