第一章提到过，Redis的散列可以让用户将多个键值对存储到一个Redis里面。从功能上来说，Redis为散列值提供了一些与字符串值相同的特性，使得散列非常适用于将一些相关的数据存储在一起。我们可以把这种数据聚集看做是关系数据库的行，或者文档数据库的文档。

本节将对最常用的散列命令进行介绍：其中包括添加和删除键值对的命令、获取所有键值对的命令、已经对键值对的值进行自增或者自减操作的命令。阅读这一节可以让读者学习到如何将数据存储到散列里面，以及这样做的好处是什么。下表展示了一部分常用的散列命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| hmget | hmget key-name key \[key ...\] | 从散列里面获取一个或多个键的值 |
| hmset | hmset key-name key value \[key value ...\] | 为散列里面的一个或多个键设置值 |
| hdel | hdel key-name key \[key ...\] | 删除散列里面的一个或多个键值对，返回成功找到并删除的键值对数量。 |
| hlen | hlen key-name | 返回散列包含的键值对数量。 |

hdel命令已经在第一章中介绍过了，而hlen命令以及用于一次读取或者设置多个键的hmget和hmset则是新出现的命令。像hmget和hmset这种批量处理多个键的命令既可以给用户带来方便，又可以通过减少命令的调用次数以及客户端与Redis之前的通信往返次数来提升Redis的性能。

示例：

```
import redis  # 导入redis包包

# 与本地redis进行链接，地址为：localhost，端口号为6379
r = redis.StrictRedis(host='localhost', port=6379)


values = {'k1': 'v1', 'k2': 'v2', 'k3': 'v3'}
#使用hmset命令可以一次将多个键值对添加多散列里面
print(r.hmset('hash-key', values))

keys = ['k2', 'k3']
#使用hmset命令可以一次获取多个键的值
print(r.hmget('hash-key', keys))


print(r.hlen('hash-key'))

print(r.hdel('hash-key','k1','k3'))
```

结果：

```
True
[b'v2', b'v3']
4
2
```

第一章介绍的hget命令和hset命令分别是hmget命令和hmset命令的单参数版本，这里的命令的唯一区别在于单参数版本每次执行只能处理一个键值对，而多参数版本的每次执行可以处理多个键值对。

下表列出了散列的其他几个批量操作命令，以及一些和字符串操作类似的散列命令。

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| hexists | hexists key-name key | 检查给定键是否存在于散列中 |
| hkeys | hkeys key-name | 获取散列包含的所有键 |
| hvals | hvals key-name | 获取散列包含的所有值 |
| hgetall | hgetall key-name | 获取散列包含的所有键值对 |
| hincrby | hincrby key-name key increment | 将键key存储的值加上整数increment |
| hincrbyfloat | hincrbyfloat key-name key increment | 将键key存储的值加上浮点数increment |

尽管有hgetall存在，但hkeys和hvals也是非常有用的：如果散列包含的值非常大，那么用户可以先使用hkeys取出散列包含的所有键，然后再使用hget一个接一个地取出键的值，从而避免因为一次获取多个大体积的值而导致服务器阻塞。

hincrby和hincrbyfloat可能会让读者回想起用于处理字符串的incrby和incrbyfloat，这两个命令拥有相同的语义，他们的不同在于hincrby和hincrbyfloat处理的是散列，而不是字符串。

实例：

```
import redis  # 导入redis包包


# 与本地redis进行链接，地址为：localhost，端口号为6379
r = redis.StrictRedis(host='localhost', port=6379)

r.delete('hash-key2')

print(r.hmset('hash-key2',{'short:':'hello','long':1000*'1'}))

print(r.hkeys('hash-key2'))

print(r.hexists('hash-key2','num'))

#和字符串一样，对散列中一个尚未存在的键执行自增操作时，Redis会将键的值当作0来处理。
print(r.hincrby('hash-key2','num'))

print(r.hexists('hash-key2','num'))
```

结果：

```
True
[b'short:', b'long']
False
1
True
```

正如前面所说，在对散列进行处理的时候，如果键值对的值的体积非常庞大，那么用户可以先使用hkeys获取散列的所有键，然后通过只获取必要的值来减少需要传输的数据量。除此之外，用户还可以像使用sismemeber检查一个元素是否存在于集合里面一样，使用hexists检查一个键是否存在于散列中。

在接下来的一节中，我们要了解的是之后的章节会经常用到的有序集合结构。

