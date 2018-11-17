# 检查硬盘写入

为了验证主服务器是否已经将写数据发送至从服务器，用户需要在向主服务器写入真正的数据之后，再向从服务器写入一个唯一的虚构值，然后通过验证虚构值是否存在于从服务器来判断写数据是否已经到达从服务器，这个操作很容易就可以实现。另一方面，判断数据是否已经被保存到硬盘里面则要困难的多。对于每秒同步一次AOF文件的Redis服务器来说，用户总是可以通过等待1秒来确保数据已经被保存到硬盘里面，但更节约时间的做法是，检查info命令的输出结果中aof_pending_bio\_fsync属性的值是否为0，如果是的话，那么就表示服务器已经将已知的所有数据都保存到硬盘里面了。在向主服务器写入数据之后，用户可以将主服务器和从服务器的连接作为参数，调用下面函数来能自动进行上述的检查操作。

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





