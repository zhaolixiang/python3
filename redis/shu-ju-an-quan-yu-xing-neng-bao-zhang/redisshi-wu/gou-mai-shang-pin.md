# 购买商品

下面的函数展示了从市场里面购买一件商品的具体方法：程序首先使用watch对市场以及买家的个人信息进行监视，然后获取买家拥有的钱数以及商品的售价，并检查买家是否有足够的钱来购买该商品。如果买家没有足够的钱，那么程序会取消事务；相反，如果买家的钱足够，那么程序首先会将买家支付的钱转移给卖家，然后将售出的商品移动至买家的包裹，并将该商品从市场中移除。当买家的个人信息或者商品买卖市场出现变化而导致WatchError移除出现时，程序进行重试，其中最大重试时间为10秒：

```
import time
import redis

def purchase_item(conn,buyerid,itemid,sellerid,lprice):
    buyer='users:%s'%buyerid
    seller='users:%s'%sellerid
    item="%s.%s"%(itemid,sellerid)

    inventory="inventory:%s"%buyerid
    end=time.time()+10
    pipe=conn.pipeline()

    while time.time()<end:
        try:
            #对商品买卖市场以及买家对个人信息进行监视
            pipe.watch("market:",buyer)

            #检查买家想要购买的商品的价格是否出现了变化
            #以及买家是否有足够的钱来购买这件商品
            price=pipe.zscore("market:",item)
            funds=int(pipe.hget(buyer,'funds'))
            if price!=lprice or price>funds:
                pipe.unwatch()
                return None

            #先将买家支付的钱转移给卖家，然后再将购买的商品移交给买家
            pipe.multi()
            pipe.hincrby(seller,"funds",int(price))
            pipe.hincrby(buyer,'funds',int(-price))
            pipe.sadd(inventory,itemid)
            pipe.zrem("market:",item)
            pipe.execute()
            return True
        except redis.exceptions.WatchError:
            #如果买家的个人信息或者商品买卖市场在交易的过程中出现了变化，那么进行重试。
            pass
    return False
```

在执行商品购买操作定位时候，程序除了需要花费大量时间来准备相关数据之外，还需要对商品买卖市场以及买家的个人信息进行监视：监视商品买卖市场是为了确保买家想要购买的商品仍然有售（或者在商品已经被其他人买走时进行提示），而监视买家的个人信息则是为了验证买家是否有足够的钱来购买自己想要的商品。

当程序确认商品仍然存在并且买家有足够的钱的时候，程序会将被购买的商品移动到买家的包裹里面，并将买家支付的钱转移给卖家。

在观察了市场上展示的商品之后，Bill（用户ID为27）决定购买Frank在市场上销售的ItemM，下图展示了购买操作执行期间，数据结构的变化：

```
watch('market:'.'users:27') #对物品买卖市场以及Bill的个人信息进行监视
```

| 键名：market  类型：zset |
| :--- |
| ItemA.4:35 |
| ItemC.7:48 |
| ItemE.2:60 |
| ItemG.3:73 |
| ItemM.17:97 |

| 键名：users:27  类型：hash |
| :--- |
| name：Bill |
| funds：125 |

| 键名：users:17  类型：hash |
| :--- |
| name：Bill |
| funds：43 |

```
#验证物品的售价是否并为改变
#以及Bill是否有足够的钱来购买该物品
price=pipe.zscore("market:","ItemM.17")
funds=int(pipe.hget("users:27",'funds'))
if price!=97 or price>funds:
```

```
pipe.sadd("inventory:27","ItemM")
pipe.zrem("market:","ItemM.17")
```

| 键名：market  类型：zset |
| :--- |
| ItemA.4:35 |
| ItemC.7:48 |
| ItemE.2:60 |
| ItemG.3:73 |

| 键名：users:27  类型：hash |
| :--- |
| name：Bill |
| funds：28 |

| 键名：users:17  类型：hash |
| :--- |
| name：Bill |
| funds：140 |

如果商品买卖市场有序集合或者Bill的个人信息在watch和exec执行之前发生了变化，那么purchase\_item\(\)将进行重试，或者在重试操作超时之后放弃此购买操作。

> 为什么Redis没有实现典型的加锁功能？
>
> 在访问以写入为目的的数据的时候关系数据会对被访问的数据进行加锁，知道事务被提交或者被回滚为止。如果有其他客户端视图对被加锁的数据行进行写入，那么该客户端将被阻塞，直到第一个事务执行完毕为止，加速在实际使用中非常有效，基本上所有关旭数据库都实现了这种加锁功能，它的缺点在于，持有锁的客户端运行越慢，等待解锁的客户端被阻塞的时间就越长。
>
> 因为加锁有可能会造成长时间的等待，所以Redis为了尽可能减少客户端的等待时间，并不会在执行watch命令时对数据进行加锁。相反的，Redis只会在数据已经被其他客户端抢先修改的情况下，通知执行了watch命令的客户端，这种做法被称为乐观锁，而关系数据库实际执行的加锁操作则被称为悲观锁。乐观锁在实际使用中同样非常有效，因为客户端永远不必花时间去等待第一个获得锁的客户端：他们只需要在自己的事务执行失败时进行重试就可以了。

这一节介绍了如何组合使用watch、multi和exec命令对多种类型的数据进行操作，从而实现游戏中的商品买卖市场。除了目前已有的商品买卖功能之外，我们还可以为整个市场添加商品拍卖和商品限时销售等功能，或者让市场支持更多不同类型的商品排序方式，又或者基于后面的技术，给市场添加更高级的搜索和过滤公布。

当有多个客户端同时对相同的数据进行操作时，正确的使用事务可以有效的防止数据错误的发生。而接下来的一节将展示在无须担心数据被其他客户端修改了的情况下，如果以更快地速度执行操作。



