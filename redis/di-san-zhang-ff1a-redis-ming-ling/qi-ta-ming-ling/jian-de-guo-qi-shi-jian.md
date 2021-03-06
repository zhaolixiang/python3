#键的过期时间
在使用Redis存储数据的时候，有些数据可能在某个时间点之后就不再有用了，用户可以使用DEL命令显示删除这些无用数据，也可以通过Redis的过期时间（expiration）特征来让一个键再给定的时限（timeout）之后自动被删除。当我们说一个键【带有生存时间】（time to live）或者一个键【会在特定时间之后过期】时，我们指的是Redis会在这个键的过期时间到达时自动删除该键。

虽然过期时间特性对于清理缓存数据非常有用，不过通常只有少数几个命令可以原子地为键设置过期时间，并且对于列表、集合、散列和有序集合这样的容器来说，键过期命令只能为整个键设置过期时间，而没办法为键里面的单个元素设置过期时间（为了解决这个问题，可以使用存储时间戳的有序集合来实现针对的那个元素的过期操作）。

本节将对那些可以在给定时限或者给定时间之后，自动删除过期键的Redis命令进行介绍。通过阅读本节，读者可以学会如何使用过期操作来自动删除过期数据并降低Redis的内存占用。

下表列出了Redis提供的用于为键设置过期时间的命令，已经查看键的过期时间的命令：

| 命令 | 示例 | 描述 |
| :--- | :--- | :--- |
| persist | persist key-name | 移除键的过期时间 |
| ttl | ttl key-name | 查看给定键距离过期还有多少秒 |
| expire | expire key-name seconds | 让给定键再指定的秒数之后过期 |
| expireat | expireat key-name timestamp | 将给定键的过期时间设置为给定的UNIX时间戳。 |
| pttl | pttl key-name | 查看给定键距离过期时间还有多少毫秒，这个命令在Redis2.6或以上版本可用， |
| pexpire | pexpire key-name milliseconds | 让给定键再指定的毫秒之后过期。这个命令在Redis2.6或以上版本可用。 |
| pexpireat | pexpireat key-name timestamp-milliseconds | 将一个毫秒级精确的UNIX时间戳设置为给定键的过期时间，这个命令在Redis2.6或以上版本可用。 |

下面代码展示了几个对键执行过期时间操作的例子：

```
import redis  # 导入redis包包
import time


# 与本地redis进行链接，地址为：localhost，端口号为6379
r = redis.StrictRedis(host='localhost', port=6379)
r.delete('trans:')

#设置一个简单的字符串值作为过期时间的设置对象
print(r.set('key','value'))
print(r.get('key'))
print(r.expire('key',2))

time.sleep(1)

#查看键距离过期还有多长时间
print(r.ttl('key'))

time.sleep(1)

#此时键已经过期，并被删除
print(r.get('key'))

```

运行结果：

```
True
b'value'
True
1
None
```



