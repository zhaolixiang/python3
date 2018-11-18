# 将商品放到市场上销售

为了将商品放到市场上进行销售，程序除了要使用multi命令和exec命令之外，还需要配合使用watch命令，有时候甚至还会用到unwatch和discard命令。在用户使用watch命令对键进行监视之后，直到用户执行exec命令的这段时间里面，如果有其他客户端抢先对任何被监视的键进行了替换、更新或删除等操作，那么当用户尝试执行exec命令的时候，事务将失败并返回一个错误（之后用户可以选择重试事务或者放弃事务）。通过使用watch、multi/exec、unwatch/discard等命令。程序可以在执行某些重要操作的时候，通过确保资金正在使用的数据没有发生变化来避免数据出错。

> 什么是discard？
>
> unwatch命令可以在watch命令执行之后，multi命令执行之前对连接进行重置（reset）；同样地，discard命令也可以在multi命令执行之后、exec命令执行之前对连接进行重置。这也就是说，用户在使用watch监视一个或多个键。接着使用multi开始一个新的事物，并将多个命令入队到事物队列之后，仍然可以通过发送discard命令来取消watch命令并清空所有已入队命令。本章展示的例子都没有用到discard，主要原因在于我们已经清楚的知道自己是否想要执行multi/exec或者unwatch，所以没有必要在这些例子里面使用discard。

在将一件商品放到市场上进行销售的时候，程序需要将被销售的商品添加到记录市场正在销售商品的有序集合里面，并且在添加操作执行的过程中，监视卖家的包裹以确保被销售的商品的确存在于卖家的包裹当中。

下面代码展示了这一操作的具体实现：

```
import time
import redis

def list_item(conn,itemid,sellerid,price):
    inventory="inventory:%s"%sellerid
    item="%s.%s"%(itemid,sellerid)
    end=time.time()+5
    pipe=conn.pipeline()

    while time.time()<end:
        try:
            #监视用户包裹发生的变化
            pipe.watch(inventory)
            #检查用户是否仍然持有将要被销售的商品
            if not pipe.sismember(inventory,itemid):
                pipe.unwatch()
                #如果指定的商品不在用户的包裹里面，那么停止对包裹键的监视并返回一个空值
                return None
            #把被销售的商品添加到商品买卖市场里面
            pipe.multi()
            pipe.zadd("market:",item,price)
            pipe.srem(inventory,itemid)
            #如果执行execute方法没有引发WatchError异常，那么说明事务执行成功，并且对包裹键的监视也已经结束。
            pipe.execute()
            return True

        except redis.exceptions.WatchError:
            #用户的包裹已经发生了变化，重试
            pass
    return False
```

上面函数的行为就和我们之前描述的一样，它首先执行一些初始化步骤，然后对卖家的包裹进行监视，验证卖家想要销售的商品是否仍然存在于卖家的包裹当中，如果是的话，函数就会将被销售的商品添加到买卖市场里面，并从卖家的包裹中移除该商品。正如函数中的while循环所示，在使用watch命令对包裹进行监视的过程中，如果包裹被更新或者修改，那么程序将接收到错误并进行重试。

下表展示了当Frank（用户ID为17）尝试以97块钱的价格销售ItemM时，list\_item\(\)函数执行的过程：

```
watch('inventory:17')    #监视包裹发生的任何变化
```

| 键名：inventory:17   类型：set |
| :--- |
| ItemL |
| ItemM |
| ItemN |

```
sismermber('inventory:17','ItemM') #确保被销售的物品仍然存在于Frank的包裹里面
```

| 键名：inventory:17  类型：set |
| :--- |
| ItemL |
| ItemM |
| ItemN |

| 键名：market  类型：zset |
| :--- |
| ItemA.4:35 |
| ItemC.7:48 |
| ItemE.2:60 |
| ItemG.3:73 |

```
#因为没有一个Redis命令可以在移除集合元素的同时，将被移除的元素改名并添加到有序集合里面
#所以这里使用了zadd和srem两个命令来实现这一操作
zadd('market','ItemM.17',97）
srem('inventory:17','ItemM')
```

| 键名：inventory:17  类型：set |
| :--- |
| ItemL |
| ItemN |

| 键名：market  类型：zset |
| :--- |
| ItemA.4:35 |
| ItemC.7:48 |
| ItemE.2:60 |
| ItemG.3:73 |
| ItemM.17:97 |

因为程序会确保用户只能销售他们自己所拥有的，所以在一般情况下，用户都可以顺利地将自己想要销售的商品添加到商品买卖市场上面，但是正如之前所说，如果用户的包裹在watch执行之后直到exec执行之前的这段时间内发送了变化，那么添加操作将执行失败并重试。

在弄懂了怎样将商品放到市场上销售之后，接下来让我们来了解一下怎样从市场上购买商品。

