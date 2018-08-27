# 1、多线程使用的必要性

在引入多线程之前，我们先来看一个非常简单的实例。

实例：

```
#单线程实例
import time

def mark(index):
    print("Mark的帅，远近闻名,第%d次传播"%index)
    #暂停一秒，不然看不到效果哦
    time.sleep(1)

if __name__=="__main__":
    for i in range(6):
        mark(i)
```

结果：按照顺序依次打印

![](/assets/1、单线程.gif)

上面是单线程显示效果，现在我们来用多线程处理一下。在这之前，我们应该要知道，thread模块是python比较底层的模块，

为了方便我们控制thread，python又使用threading模块对thread进行了封装，下面就用到了threading模块。

实例：

```
#多线程实例
import time
import threading

def mark(index):
    print("Mark的帅，远近闻名,第%d次传播"%index)
    #暂停一秒，不然看不到效果哦
    time.sleep(1)

if __name__=="__main__":
    for i in range(6):
        #定义子线程
        t=threading.Thread(target=mark,args=(i,))
        #启动子线程
        t.start()
```

效果：

![](/assets/2、多线程.gif)

看到效果了，原来的单线程，顺序执行，至少需要6s，而使用多线程，差不多1秒多一点就完成，可见运行效率的差距，这也是我们为什么要使用多线程的原因。

# 2、主线程会等待所有子线程执行完成才结束

要验证这一点很简单，直接看代码：

```
#主线程会等待所有子线程执行完成才结束
import time
import threading

def mark():
    #暂停3秒
    time.sleep(3)
    print("Mark的帅，远近闻")

if __name__=="__main__":
    print("程序开始执行了")
    # 定义子线程
    t = threading.Thread(target=mark)
    # 启动子线程
    t.start()
    print("单线程程序到这里主线程就会结束了，多线程呢，看看吧")
```

效果：

![](/assets/3、主线程会等待所有子线程执行完成才结束.gif)

