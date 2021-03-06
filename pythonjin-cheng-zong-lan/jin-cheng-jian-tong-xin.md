# 5、进程间通信

multiprocessing模块支持的进程间通信主要有两种：管道和队列。一般来说，发送较少的大对象比发送大量的小对象要好。

### Queue队列

底层使用管道和锁，同时运行支持线程讲队列中的数据传输到底层管道中，来实习进程间通信。

* ##### 语法：

```
Queue([maxsize])
创建共享队列。使用multiprocessing模块的Queue实现多进程之间的数据传递。Queue本身是一个消息队列，
maxsize是队列运行的最大项数，如果不指定，则不限制大小。
```

* ##### 常用方法

```
q.close()：关闭队列，不再向队列中添加数据，那些已经进入队列的数据会继续处理。q被回收时将自动调用此方法。

q.empty():如果调用此方法时，队列为null返回True，单由于此时其他进程或者线程正在添加或删除项，
所以结果不可靠，而且有些平台运行该方法会直接报错，我的mac系统运行该方法，直接报错。

q.full():如果调用此方法时，队列已满，返回True，同q.empty()方法，结果不可靠。

q.get([block,timeout])：返回q中的一个项，block如果设置为True,如果q队列为空，该方法会阻塞（就是不往下运行了，处于等待状态），
直到队列中有项可用为止，如果同时页设置了timeout，那么在改时间间隔内，都没有等到有用的项，就会引发Queue.Empty异常。
如果block设置为false，timeout没有意义，如果队列为空，将引发Queue.Empt异常。

q.get_nowait():等同于q.get(False)

q.put(item,block,timeout):将item放入队列，如果此时队列已满：
如果block=True，timeout没有设置，就会阻塞，直到有可用空间为止。
如果block=True，timeout也设置，就会阻塞到timeout，超过这个时间会报Queue.Full异常。
如果block=False，timeout设置无效，直接报Queue.Full异常。

q.put_nowait(item):等同于q.put(item,False)

q.qsize():返回当前队列项的数量，结果不可靠，而且mac会直接报错：NotImplementedError。
```

* ##### 实例1：验证：put方法会阻塞

实例：

```
#验证：put方法会阻塞
from multiprocessing import Queue
queue=Queue(3)#初始化一个Queue队列，可以接受3个消息
queue.put("我是第1条信息")
queue.put("我是第2条信息")
queue.put("我是第3条信息")

print("插入第4条信息之前")
queue.put("我是第4条信息")
print("插入第4条信息之后")
```

效果：程序会一直阻塞，最后一句输永远也不会输出。

![](/assets/6、验证：put方法会阻塞.gif)

* ##### 实例2：closse方法、get方法、put方法简单使用：多进程访问同一个Queue

代码：

```
#closse方法、get方法、put方法简单使用：多进程访问同一个Queue
from multiprocessing import Queue,Process
import time,os
#参数q就是Queue实例
def mark(q,interval):
    time.sleep(interval)
    # 打印信息
    print("进程%d取出数据："%os.getpid()+queue.get(True))


if __name__=="__main__":
    queue = Queue(3)  # 初始化一个Queue队列，可以接受3个消息
    queue.put("我是第1条信息")
    queue.put("我是第2条信息")
    queue.put("我是第3条信息")

    p1=Process(target=mark,args=(queue,1))
    p2=Process(target=mark,args=(queue,2))
    p3=Process(target=mark,args=(queue,3))

    p1.start()
    p2.start()
    p3.start()
    # 关闭队列，不再插入信息
    queue.close()
    # 下面插入会导致异常
    # queue.put("我是第4条信息")

    # 打印第1条信息
    print("程序语句执行完成")
```

效果

 ![](/assets/7、closse方法、get方法、put方法简单使用：多进程访问同一个Queue.gif)

### JoinableQueue队列

创建可连接的共享进程队列，可以看做还是一个Queue，只不过这个Queue除了Queue特有功能外，允许项的消费者通知项的生产者，项已经处理成功。该通知进程时使用共享的信号和条件变量来实现的。

JoinableQueue实例除了与Queue对象相同的方法外，还具有下列方法：

```
q.task_done():消费者使用此方法发送信号，表示q.get()返回的项已经被处理。
注意⚠️：如果调用此方法的次数大于队列中删除的项的数量，将引发ValueError异常。

q.join():生产者使用此方法进行阻塞，直到队列中所有的项都被处理完成，即阻塞将持续到队列中的每一项均调用q.task_done()方法为止。
```

代码实例：

```
#利用JoinableQueue实现生产者与消费者，并且加入了哨兵，来监听生产者的要求
from multiprocessing import JoinableQueue,Process
import time

#参数q为JoinableQueue队列实例
def mark(q):
    #循环接受信息,一直运行，这也下面为什么要将它设为后台进程的原因，必须保证当主线程退出时，它可以退出
    while True:
        value = q.get()
        print(value)  # 实际开发过程中，此处一般用来进行有用的处理
        # 消费者发送信号：任务完成（此处实例的任务就是打印一下下）
        q.task_done()
        #我来方便看出效果，特意停留1s
        time.sleep(1)

        #使用哨兵，监听生产者的消息，此处通过判断value是否为None来判断传递的消息
        if value==None:
            #执行哨兵计划后，后面的语句都不会输出
            break




if __name__=="__main__":
    #实例化JoinableQueue
    q=JoinableQueue()

    #定义消费者进程
    p=Process(target=mark,args=(q,))
    #将消费者线程设置为后台进程，随创建它的进程（此处是主进程）的终止而终止
    #也就是当它的创建进程（此处是主现场）意外退出时，它也会跟随一起退出。
    #并且后台进程无法创建新的进程
    p.daemon=True

    #启动消费者进程
    p.start()

    #模拟生产者，生产多个项
    for xx in range(5):
        print(xx)
        #当xx==3时去执行哨兵计划
        if xx==3:
            print("我用哨兵计划了")
            q.put(None)
            print("哨兵计完美执行")
        q.put("第%d条消息"%xx)


    #等待所有项都处理完成再退出，由于使用了哨兵计划，队列没有完全执行，所以会一直卡在这个位置
    q.join()

    print("程序真正退出了")
```

效果：

![](/assets/利用JoinableQueue实现生产者与消费者，并且加入了哨兵，来监听生产者的要求.gif)

### 管道

除了使用队列来进行进程间通信，还可以使用管道来进行消息传递。

* ##### 语法：

```
(connection1,connection2)=Pipe([duplex])
在进程间创建一条管道，并返回元祖(connection1,connection2),其中connection1、connection2表示两端的Connection对象。
默认情况下，duplex=True，此时管道是双向的，如果设置duplex=false，connection1只能用于接收，connection2只能用于发送。
注意：必须在多进程创建之前创建管道。
```

* ##### 常用方法：

```
connection.close() :关闭连接，当connection被垃圾回收时，默认会调用该方法。

connection.fileno() :返回连接使用的整数文件描述符

connection.poll([timeout]):如果连接上的数据可用，返回True，timeout为等待的最长时间，如果不指定，该方法将立刻返回结果。
如果指定为None，该方法将会无限等待直到数据到达。

connection.send(obj):通过连接发送对象，obj是与序列号兼容的任意对象。

connection.send_bytes(buffer[,offset,size]):通过连接发送字节数据缓冲区，buffer是支持缓冲区的任何对象。
offset是缓冲区的字节偏移量，而size是要发送的字节数。

connection.recv():接收connection.send()方法返回的对象。如果连接的另一端已经关闭，再也不会存在任何数据，
该方法将引起EOFError异常。

connection.recv_bytes([maxlength]):接收connection.send_bytes()方法发送的一条完整字节信息，maxlength为可以接受的
最大字节数。如果消息超过这个最大数，将引发IOError异常，并且在连接上无法进一步读取。如果连接的另一端已经关闭，
再也不会有任何数据，该方法将引发EOFError异常。

connection.recv_bytes_into(buffer[,offset]):接收一条完整的字节信息，兵把它保存在buffer对象中，
该对象支持可写入的缓冲区接口（就是bytearray对象或类似对象）。
offset指定缓冲区放置消息的字节偏移量。返回值是收到的字节数。如果消息长度大于可用的缓冲区空间，将引发BufferTooShort异常。
```

* ##### 实例1：理解管道的生产者与消费者

示意图：

![](/assets/管道：生产者与消费者示意图.jpg)

代码：

```
#理解管道的生产者与消费者
from multiprocessing import Pipe, Process
import time

def mark(pipe):
    #接受参数
    output_p, input_p = pipe
    print("mark方法内部调用input_p.close()")
    #消费者（子进程）此实例只接收，所以把输入关闭
    input_p.close()

    while True:
        try:
            item = output_p.recv()
        except EOFError:
            print("报错了")
            break
        print(item)
        time.sleep(1)
    print("mark执行完成")


if __name__ == "__main__":
    #必须在多进程创建之前，创建管道,该管道是双向的
    (output_p, input_p) = Pipe()#创建管道
    #创建一个进程，并把管道两端都作为参数传递过去
    p = Process(target=mark, args=((output_p, input_p),))
    #启动进程
    p.start()
    #生产者（主进程）此实例只输入，所以关闭输出（接收端）
    output_p.close()

    for item in list(range(5)):
        input_p.send(item)
    print("主方法内部调用input_p.close()()")
    #关闭生产者（主进程）的输入端
    input_p.close()
```

效果图：

![](/assets/8、理解管道的生产者与消费者.gif)

* ##### 实例2：利用管道实现多进程协作：子线程计算结果，返回给主线程

 代码：

```
#利用管道实现多进程协作：子线程计算结果，返回给主线程
from multiprocessing import Pipe, Process

def mark(pipe):
    #接受参数
    server_p, client_p = pipe
    #消费者（子进程）此实例只接收，所以把输入关闭
    client_p.close()

    while True:
        try:
            x,y = server_p.recv()
        except EOFError:
            print("报错了")
            break
        result=x+y
        server_p.send(result)
    print("mark执行完成")


if __name__ == "__main__":
    #必须在多进程创建之前，创建管道,该管道是双向的
    (server_p, client_p) = Pipe()#创建管道
    #创建一个进程，并把管道两端都作为参数传递过去
    p = Process(target=mark, args=((server_p, client_p),))
    #启动进程
    p.start()
    #生产者（主进程）此实例只输入，所以关闭输出（接收端）
    server_p.close()

    #发送数据
    client_p.send((4,5))
    #打印接受到的数据
    print(client_p.recv())

    client_p.send(("Mark", "大帅哥"))
    # 打印接受到的数据
    print(client_p.recv())





    #关闭生产者（主进程）的输入端
    client_p.close()
```

结果：

```
9
Mark大帅哥
报错了
mark执行完成
```



