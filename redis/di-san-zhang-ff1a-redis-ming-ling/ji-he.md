Redis的集合以无序的方式来存储多个各不相同的元素，用户可以快速地对集合执行添加元素操作、移除元素操作、以及检查一个元素是否存在于集合里。本节将对最常用的集合命令进行介绍，包括：插入命令、移除命令、将元素从一个集合移动到另一个集合的命令、以及对多个集合执行交集运算、并集运算、差集运算的命令。

下表展示了其中一部分最常用的集合命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| sadd | sadd key-name item \[item ...\] | 将一个或多个元素添加到集合里面，并返回被添加元素当中原本并不存在于集合里面的元素数量 |
| srem | srem key-name item \[item ...\] | 从集合里面移除一个或多个元素，并返回被移除元素的数量 |
| sismember | sismember key-name item | 检查元素item是否存在于集合key-name里 |
| scard | scard key-name | 返回集合包含的元素的数量 |
| smembers | smembers key-name | 返回集合包含的所有元素 |
| srandmember | srandmember key-name \[count\] | 从集合里面随机地返回一个或多个元素。当count为正数时，命令返回的随机元素不会重复；当count为负数时，命令返回的随机元素可能会出现重复。 |
| spop | spop key-name | 随机地移除集合中的一个元素，并返回被移除的元素。 |
| smove | smove source-key dest-key item | 如果集合source-key包含元素item，那么从集合source-key里面移除元素item，并将元素item添加到集合dest-key中；如果item被成功移除，那么命令返回1，否则返回0 |

### Redis常用集合命令使用实例

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)

r.delete('set-key')

#sadd命令会将那些目前并不存在于集合里面的元素添加到集合里面，并返回被添加元素的数量
print(r.sadd('set-key','a','b','c'))

#srem命令返回的是被移除元素的数量
print(r.srem('set-key','c','d'))
print(r.srem('set-key','c','d'))


#查看集合包含的元素数量
print(r.scard('set-key'))

#获取集合包含的所有元素
print(r.smembers('set-key'))

#可以很容易地将元素从一个集合移动到另一个集合
print(r.smove('set-key','set-key2','a'))
#在执行smove命令时，如果用户想要移动的元素不存在于第一个集合里，那么移动操作就不会执行，返回False
print(r.smove('set-key','set-key2','a'))


print(r.smembers('set-key2'))
```

运行结果：

```
3
1
0
2
{b'a', b'b'}
True
False
{b'a'}
```

通过使用上面展示的命令，我们可以将各不相同的元素添加到集合里面，但集合真正厉害的地方在于组合和关联多个集合，下表展示了相关的命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| sdiff | sdiff key-name \[key-name ...\] | 返回那些存在于第一个集合、但不存在于其它集合中的元素（数学上的差集运算） |
| sdiffstore | sdiffstore dest-key key-name \[key-name ...\] | 将那些存在于第一个集合但不存在于其他集合中的元素（数学上的差集运算）存储到dest-key键里面 |
| sinter | sinter key-name \[key-name ...\] | 返回那些同时存在于所有集合中的元素（数学上的交集运算） |
| sinterstore | sinterstore dest-key key-name \[key-name ...\] | 将那些同时存在于所有集合的元素（数学上的交集运算）存储到dest-key键里面 |
| sunion | sunion key-name \[key-name ...\] | 返回那些至少存在于一个集合中的元素（数学上的并集计算） |
| sunionstore | sunionstore dest-key key-name \[key-name ...\] | 将那些至少存在于一个集合中的元素（数学上的并集计算）存储到dest-key键里面 |

这些命令分别是并集运算、交集运算和差集运算这3个基本集合操作的”返回结果“版本和”存储结果“版本。

示例：

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)

r.delete('set-key1')
r.delete('set-key2')

#首先将这一些元素添加到两个集合里面
print(r.sadd('set-key1','a','b','c','d'))
print(r.sadd('set-key2','c','d','e','f'))

#计算出从第一个集合里面移除第二个集合包含的所有元素的结果
print(r.sdiff('set-key1','set-key2'))

#计算出同时存在于两个集合里面的所有元素
print(r.sinter('set-key1','set-key2'))

#计算出两个集合包含的所有元素
print(r.sunion('set-key1','set-key2'))


```

运行结果：

```
4
4
{b'b', b'a'}
{b'c', b'd'}
{b'c', b'e', b'd', b'f', b'a', b'b'}
```

和Python的集合相比，Redis的集合除了可以被多个客户端远程地进行访问以外，其他的语义和功能基本都是相同的。

接下来的一节将对Redis的散列处理命令进行介绍，这些命令允许用户将多个相关的键值对存储在一起，以便执行获取操作和更新操作。

