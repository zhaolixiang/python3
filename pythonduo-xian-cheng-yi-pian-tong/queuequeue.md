> queue模块实现了各种【多生产者-多消费者】队列。可用于在执行的多个线程之间安全的交换信息。
>
> queue模块定义了3种不同的队列类。

### 3种不同的队列类

```
q=Queue(maxsize):创建一个FIFO(first-in first-out,先进先出)队列。maxsize是队列中金额以放入的项的最大数量。
如果省略maxsize参数或将它置为0，队列大小将无穷大。

q=LifoQueue(maxsize)：创建一个LIFO（last-in first-out，后进先出）队列(栈)。

q=PriorityQueue(maxsize)：创建一个优先级队列，其中项按照优先级从低到高依次排好。
使用这种队列时，项应该是(priority,data)形式的元组，其中priority时一个标识优先级的数字。
```

### 常用方法

```
q.size()：返回队列的正确大小。因为其他线程可能正在更新此队列，所以此方法的返回数字不可靠。

q.empty()：如果队列为空，返回True，否则返回False。

q.full()：如果队列已满，返回True，否则返回False。

q.put(item,block,timeout)：将item放入队列。
如果block设为True（默认值），调用者将被阻塞直到队列中出现可用的空闲位置为止。
如果block设为False，队列满时此方法将引发Full异常。

q.put_nowait(item):等价于q.put(item,False)

q.get(block,timeout):从队列中删除一项，然后返回这个项。
如果block设为True（默认值），调用者将阻塞，直到队列中出现可用的空闲为止。
如果block设为False，队列为空时将引发Empty异常。
timeout提供可选的超时值，单位为秒，如果超时，将引发Empty异常。

q.get_nowait()：等价于get(0)

q.task_done():在队列种数据的消费者用来指示对于项的处理已经结束。如果使用此方法，那么从队列中删除的每一项都应该调用一次。

q.join()
```



