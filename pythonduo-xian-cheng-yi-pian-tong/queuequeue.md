> queue模块实现了各种【多生产者-多消费者】队列。可用于在执行的多个线程之间安全的交换信息。
>
> queue模块定义了3种不同的队列类。

```
Queue(maxsize):创建一个FIFO(first-in first-out,先进先出)队列。maxsize是队列中金额以放入的项的最大数量。
如果省略maxsize参数或将它置为0，队列大小将无穷大。

LifoQueue(maxsize)：创建一个LIFO（last-in first-out，后进先出）队列(栈)。

PriorityQueue(maxsize)
```



