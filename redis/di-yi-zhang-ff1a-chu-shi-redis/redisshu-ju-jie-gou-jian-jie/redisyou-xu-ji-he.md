### 有序集合

有序集合和散列一样，都用于存储键值对：有序集合的键被称为【成员】（member），每个成员都是各不相同的；而有序集合的值则被称为【分值】（score），分值必须为浮点数。

有序集合是Redis里面唯一一个既可以根据成员访问元素（这一点和散列表一样），又可以根据分值以及分值的排列顺序来访问元素的结构。

下表列出了有序集合的命令

| 命令 | 行为 |
| :--- | :--- |
| zadd | 将一个带有给定分值的成员添加到有序集合里面 |
| zrange | 根据元素在有序排列中所处的位置，从有序集合里面获取多个元素。 |
| zrangebyscore | 获取有序集合在给定分值范围内的所有元素 |
| zrem | 如果给定元素存在于有序集合，那么移除这个成员。 |

##### 实例：

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)

r.delete("zset-key")

print(r.zadd("zset-key",7,"member1")) #返回添加元素的数量
print(r.zadd("zset-key",9,"member2")) #返回添加元素的数量
print(r.zadd("zset-key",8,"member3")) #返回添加元素的数量
print(r.zadd("zset-key",7,"member3")) #返回添加元素的数量,如果存在就覆盖值


print(r.zrange("zset-key",0,-1)) #h获取有序集合所包含的所有元素，多个元素或安装分值大小进行排序

print(r.zrangebyscore("zset-key",0,7,withscores=memoryview)) #根据分值来获取集合中的一部分元素

print(r.zrem("zset-key","member1"))  #返回移除元素的数量

print(r.zrange("zset-key",0,-1,withscores=memoryview)) #h获取有序集合所包含的所有元素，多个元素或安装分值大小进行排序




```

结果：

```

```



