# 检查硬盘写入

为了验证主服务器是否已经将写数据发送至从服务器，用户需要在向主服务器写入真正的数据之后，再向从服务器写入一个唯一的虚构值，然后通过验证虚构值是否存在于从服务器来判断写数据是否已经到达从服务器，这个操作很容易就可以实现。另一方面，判断数据是否已经被保存到硬盘里面则要困难的多。对于每秒同步一次AOF文件的Redis服务器来说，用户总是可以通过等待1秒来确保数据已经被保存到硬盘里面，但更节约时间的做法是，检查info命令的输出结果中aof\_pending\_bio\_fsync属性的值是否为0，如果是的话，那么就表示服务器已经将已知的所有数据都保存到硬盘里面了。在向主服务器写入数据之后，用户可以将主服务器和从服务器的连接作为参数，调用下面函数来能自动进行上述的检查操作。

```
import time
import uuid


def wait_for_sync(mconn,sconn):
    identifier=str(uuid.uuid4())
    #将令牌添加至主服务器
    mconn.zadd('sync:wait',identifier,time.time())

    #如果有必要的话，等待从服务器完成同步
    while not sconn.info()['master_lin_status']!='up':
        time.sleep(.001)

    #等待从服务器接收数据更新
    while not sconn.zscore('sync:wait',identifier):
        time.sleep(.001)

    deadline=time.time()+1.01
    #最多只等待一秒
    while time.time()<deadline:
        #检查数据更新是否已经被同步到了硬盘
        if sconn.info()['aof_pending_bio_fsync']==0:
            break
    #清理刚刚创建的新令牌以及之前可能留下的旧令牌
    mconn.zrem('sync:wait',identifier)
    mconn.zremrangebyscore('sync:wait',0,time.time()-900)
```

> info命令中的其他信息：
>
> info命令提供了大量的与Redis服务器当前状态相关的信息，比如内存占用量、客户端连接数、每个数据库包含的键的数量、上一次创建快照文件之后执行的命令数量、等待。总的来说，info命令对于了解Redis服务器的综合状态非常有帮助，网上有很多资源对info命令进行了详细的介绍。

为了确保操作可以正确执行，上面函数会首先确认从服务器已经连接上主服务器，然后检查自己添加到等待同步有序集合里面的值是否已经存在于从服务器。在发现值已经存在于从服务器之后，函数会检查从服务器写入缓冲区的状态，并在一秒之内，等待从服务器将缓冲区中的所有数据写入硬盘里面。虽然函数最多会花费1秒来等待同步完成，但实际上大部分同步都会在很短的时间完成。最后，在确认数据已经被保存到硬盘之后，函数会执行一些清理操作。

通过同步使用复制和AOF持久化，用户可以增强Redis对于系统崩溃的抵抗能力。



