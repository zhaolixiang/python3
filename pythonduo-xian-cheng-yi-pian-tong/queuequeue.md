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

q.empty()：如果队列为空，返回True，否则返回False

q.full()

q.put(item,block,timeout)
```



