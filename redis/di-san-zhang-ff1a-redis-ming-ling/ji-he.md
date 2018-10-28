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



