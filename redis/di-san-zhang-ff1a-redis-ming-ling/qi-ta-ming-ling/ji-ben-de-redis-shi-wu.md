# 基本的Redis事务

有时候为了同时处理多个结构，我们需要向Redis发送多个命令。尽管Redis有几个可以在两个键之间复制或者移动元素，但却没有那种可以在两个不同类型之间移动元素的命令（虽然可以使用zunionstore命令将元素从一个集合复制到一个有序集合）。为了对相同或者不同类型的多个键执行操作，Redis有5个命令可以让用户在不被打断（interruption）的情况下对多个键执行操作，它们分别是watch、multi、exec、unwatch、discard。

这一节中介绍最基本的Redis事务用法，其中只会用到multi命令和exec命令。

### 什么是Redis的基本事务

Redis的基本事务需要用到multi命令和exec命令，这种事务可以让一个客户端在不被其他客户端打断的情况下执行多个命令。和关系数据库那种可以在执行的过程中进行回滚（rollback）的事务不同，在Redis里面，被multi命令和exec命令包围的所有命令会一个接一个的执行，直到所有命令都执行完毕。当一个事务执行完毕之后，Redis才会处理其他客户端的命令。

要在Redis里面执行事务，我们首先需要执行multi命令，然后输入那些我们想要在事务里面执行的命令，最后再执行exec命令。当Redis从一个客户端那里接受到multi命令时，Redis会将这个客户端之后发送的所有命令都放入到一个队列里面，直到这个客户端发送exec命令为止，然后Redis就会在不被打断的情况下，一个接一个地执行存储在队列里面的命令。从语义上来说，Redis事务在Python客户端上面是由流水线（pipelien）实现：对连接对象用pipeline\(\)方法将创建一个事务，在一切正常的情况下，客户端会自动地使用multi和exec包裹起用户输入的多个命令。此处，为了减少Redis与客户端之间的通信往返次数，提升执行多个命令时的性能，Python的Redis客户端会存储起事务包含的多个命令，然后在事务执行时一次性地将所有命令都发送给Redis。

跟介绍publish命令和subscribe命令时的情况一样，要展示事务执行结果，最简单的方法就是将事务放到线程里执行。

下面代码展示了在没有使用事务的情况下，执行并行（parallel）自增操作的结果：

```
import redis  # 导入redis包包
import time,threading


# 与本地redis进行链接，地址为：localhost，端口号为6379
r = redis.StrictRedis(host='localhost', port=6379)
r.delete('sort-input')

def notrans():
    #对'notrans:'计数器执行自增操作并打印操作的执行结果
    print(r.incr('notrans:'))
    #等待100毫秒
    time.sleep(.1)
    #对'notrans:'计数器执行自减操作。
    r.incr('notrans:',-1)


if __name__ == '__main__':
    if 1:
        #启动3个线程来执行没有被事务包裹的自增、休眠和自减操作
        for i in range(3):
            threading.Thread(target=notrans).start()
        #等待500毫秒，让操作有足够的时间完成
        time.sleep(.5)

```

结果：

```
1
2
3
```

因为没有使用事务，所以三个线程都可以在执行自减操作之前，对notrans：计数器执行自增操作。虽然代码里面通过休眠100毫秒放大了潜在问题，但如果我们确实需要在不受其它命令干扰的情况下，对计数器执行自增操作和自减操作，那么我们就不得不解决这个潜在问题。

下面代码使用事务来执行相同的操作：

```

```

