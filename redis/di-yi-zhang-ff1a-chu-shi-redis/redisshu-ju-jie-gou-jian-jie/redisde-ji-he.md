### 集合

Redis的集合和列表都可以存储多个字符串，他们的不同在于，列表可以存储多个相同的字符串，而集合则通过使用散列表来保证自己存储的每个字符串都是各不相同的。

因为Redis的集合使用无序（unordered）方式存储元素，所以不能像使用列表那样，将元素推入集合的某一端，或者从集合的某一端弹出元素。

下表列出了集合的命令：

| 命令 | 行为 |
| :--- | :--- |
| sadd | 将给定元素添加到集合 |
| smembers | 返回集合包含的所有元素，如果集合包含的元素非常多，那么该命令的执行速度可能会很慢 |
| sismember | 检查给定元素是否存在于集合中 |
| srem | 如果给定集合存在于集合中，那么移除这个元素 |

##### 实例：

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)

r.delete("set-key")

print(r.sadd("set-key","item1")) #向集合中添加新的元素，语句返回1表示这个元素被成功添加到集合中，返回0则表示这个元素已经存在集合中
print(r.sadd("set-key","item2")) #向集合中添加新的元素，语句返回1表示这个元素被成功添加到集合中，返回0则表示这个元素已经存在集合中
print(r.sadd("set-key","item3")) #向集合中添加新的元素，语句返回1表示这个元素被成功添加到集合中，返回0则表示这个元素已经存在集合中

print(r.sadd("set-key","item3")) #向集合中添加新的元素，语句返回1表示这个元素被成功添加到集合中，返回0则表示这个元素已经存在集合中

print(r.smembers("set-key"))  #返回集合包含的所有元素

print(r.sismember("set-key","item1")) #判断一个元素是否存在于集合汇总
print(r.sismember("set-key","item4")) #判断一个元素是否存在于集合汇总

print(r.srem("set-key","item1"))  #如果指定元素在集合中存在，就移除，结果返回移除元素的数量
print(r.srem("set-key","item4"))  #如果指定元素在集合中存在，就移除，结果返回移除元素的数量

print(r.smembers("set-key"))  #返回集合包含的所有元素


```

结果：

```
1
1
1
0
{b'item2', b'item3', b'item1'}
True
False
1
0
{b'item2', b'item3'}
```



