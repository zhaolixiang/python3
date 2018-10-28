和散列存储着键与值之间的映射类似，有序集合也存储着成员与分值之间的映射，并且提供了分值处理命令，已经根据分值大小有序的获取（fetch）和扫描（scan）成员和分值的命令。本书曾在第一章使用有序集合实现过基于发布时间排序的文章列表和基于投票数量排序的文章列表，还在第二章使用有序集合存储过cookie的过期时间。

> 这些分值在Redis中以IEEE754双精度点数的格式存储。

本节将对操作有序集合的命令进行介绍，其中包括向有序集合添加新元素的命令、更细已有元素的命令、以及对有序集合进行交集运算和并集运算的命令。

下表展示了一部分常用的有序集合命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| zadd | zadd key-name score memeber \[score member ...\] | 将带有给定分值的成员添加到有序集合里面 |
| zrem | zrem key-name member \[member ...\] | 从有序集合里面移除给定的成员，并返回被移除成员的数量 |
| zcard | zcard key-name | 返回有序集合包含的成员数量 |
| zincrby | zincrby key-name increment member | 将merber成员的分值加上increment |
| zcount | zcount key-name min max | 返回分值介于min和max之间的成员数量 |
| zrank | zrank key-name member | 返回成员member有序集合中的排名。 |
| zscore | zsore key-name member | 返回成员member的分值 |
| zrange | zrange key-name start stop \[WITHSCORES\] | 返回有序集合中排名介于start和stop之间的成员，如果给定了可选的WITHSCORES选项，那么命令会将成员的分值也一并返回。 |

实例：

```
import redis  # 导入redis包包


# 与本地redis进行链接，地址为：localhost，端口号为6379
r = redis.StrictRedis(host='localhost', port=6379)

r.delete('zset-key')

print(r.zadd('zset-key',3,'a',2,'b',1,'c'))

print(r.zcard('zset-key'))

print(r.zincrby('zset-key','c',3))

print(r.zscore('zset-key','b'))

print(r.zrank('zset-key','c'))

#返回介于0-3之间的成员数量
print(r.zcount('zset-key',0,3))

print(r.zrem('zset-key','b'))

print(r.zrange('zset-key',0,-1,withscores=True))
```

结果：

```
3
3
4.0
2.0
2
2
1
[(b'a', 3.0), (b'c', 4.0)]
```

因为zadd、zrem、zincrby、zscore和zrange都已经在第一章和第二章介绍过了，所以读者应该不会对它们感到陌生。zcount命令和其他命令不太相同，它主要拥挤计算分值在给定范围内的成员数量。

下表展示了另外一些非常有用的有序集合命令：

有序集合的范围型数据获取命令和范围型数据删除命令，以及并集命令和交集命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| zrevrank | zrevrank key-name merber | 返回有序集合里面成员member的排名，成员按照分值从大到小排列 |
| zrevrange | zrevrange key-name start stop \[WITHSCORES\] | 返回有序集合给定排名范围内的成员，成员按照分值从大到小排列 |
| zrangebysocre | zrangebyscore by min max \[WITHSCORES\] \[LIMIT offset count\] | 返回有序集合中，分值介于min和max之间的所有成员。 |
| zrevrangebyscore | zrevrangebyscore key max min \[WITHSCORES\] \[LIMIT offset count\] | 获取有序集合中分值介于min和max之间的所有成员，并按照分值从大到小的顺序来返回它们。 |
| zremrangebyrank | zremrangebyrank key-name start stop | 移除有序集合中排名介于start和stop之间的所有成员。 |
| zremrangebyscore | zremrangebyscore key-name min max | 移除有序集合中分值介于min和max之间的所有成员。 |
| zinterstore | zinterstore dest-key key-count key \[key ...\] | 对给定的有序集合执行类似于集合的交集运算。 |
| zunionstore | zunionstore dest-key key-count key \[key ...\] \[weights weight \[weight ...\]\]\[aggregate sum\|min\|max\] | 对给定的有序集合执行类似于集合的并集运算。 |

上表展示的命令里面，有几个是之前没介绍过的新命令。除了使用逆序来处理有序集合之外，zrev\*命令的工作方式和相对应的非逆序命令的工作方式完全一样（逆序就是指元素按照从大到小地排列）。

实例：

```
import redis  # 导入redis包包


# 与本地redis进行链接，地址为：localhost，端口号为6379
r = redis.StrictRedis(host='localhost', port=6379)

r.delete('zset-key1')
r.delete('zset-key2')
r.delete('zset-key-i')
r.delete('zset-key-u')
r.delete('zset-key-u2')
r.delete('set-1')

#首先创建两个有序集合
#a  b   c   d
#1  2   3
#   4   1   0
print(r.zadd('zset-key1',1,'a',2,'b',3,'c'))
print(r.zadd('zset-key2',4,'b',1,'c',0,'d'))


#zinterstore和zunionstore默认使用的聚合函数为sum，这个函数会把各个有序集合的成员的分值都加起来
print(r.zinterstore('zset-key-i',['zset-key1','zset-key2']))

print(r.zrange('zset-key-i',0,-1,withscores=True))

#用户可以在执行并集运算和交集运算的时候传入不同的聚合函数，共有sum、min、max三个聚合函数可选。
print(r.zunionstore('zset-key-u',['zset-key1','zset-key2'],aggregate='min'))

print(r.zrange('zset-key-u',0,-1,withscores=True))

print(r.sadd('set-1','a','d'))

#用户可以把集合作为输入传给zinterstore和zunionstore，命令会将集合看作是成员分值全为1的有序集合来处理。
print(r.zunionstore('zset-key-u2',['zset-key1','zset-key2','set-1']))

print(r.zrange('zset-key-u2',0,-1,withscores=True))
```

结果：

```
3
3
2
[(b'c', 4.0), (b'b', 6.0)]
4
[(b'd', 0.0), (b'a', 1.0), (b'c', 1.0), (b'b', 2.0)]
2
4
[(b'd', 1.0), (b'a', 2.0), (b'c', 4.0), (b'b', 6.0)]
```



