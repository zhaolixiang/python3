在之前我们介绍过，Redis的列表允许用户从序列的两端推入或者弹出元素、获取列表元素、以及执行各种常见的列表操作。除此之外，列表还可以用来存储任务信息、最近浏览过的文章、常用联系人信息。

本节将对列表这个由多个字符串组成的有序序列结构进行介绍，并展示一些最常用的列表处理命令。

下表展示了常用的列表命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| rpush | rpush key-name value \[value ...\] | 将一个或多个值推入列表的右端 |
| lpush | lpush key-namr value \[value ...\] | 将一个或多个值推入列表的左端 |
| rpop | rpop key-name | 移除并返回列表最右端的元素 |
| lpop | lpop key-name | 移除并返回列表最左端的元素 |
| lindex | lindex key-name offset | 返回列表中偏移量为offset的元素 |
| lrange | lrange key-name start end | 返回列表从start偏移量到end偏移量范围内的所有元素，其中偏移量为start和偏移量为end的元素也会包含在被返回的元素之内。 |
| ltrim | ltrim key-name start end | 对列表进行修剪，只保留从strat偏移量到end偏移量范围内的元素，其中偏移量为start何偏移量为end的元素也会被保留。 |

之前已经对列表的几个推入和弹出操作进行了简单的介绍，所以我们应该不会对上面的命令陌生。

### 列表推入、弹出操作实例

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)
r.delete('list-key')

#推入操作完成之后会返回列表当前的长度
#从语义上来说，列表的左端为开头，右端为结尾
print(r.rpush('list-key','last'))

print(r.lpush('list-key','first'))

print(r.rpush('list-key','new last'))

print(r.lrange('list-key',0,-1))

#通过重复的弹出列表左端的元素，可以按照从左到右的顺序来获取列表中的元素
print(r.lpop('list-key'))
print(r.lpop('list-key'))

print(r.lrange('list-key',0,-1))

#可以同时推入多个元素
print(r.lpush('list-key','a','b','c'))
print(r.lrange('list-key',0,-1))

#可以从列表的左端、右端或者左右两端删减任意数量的元素
print(r.ltrim('list-key',2,-1))
print(r.lrange('list-key',0,-1))
```

运行结果：

```
1
2
3
[b'first', b'last', b'new last']
b'first'
b'last'
[b'new last']
4
[b'c', b'b', b'a', b'new last']
True
[b'a', b'new last']
```

这个实例里面第一次使用到了ltrim命令，聚合使用ltrim和lrange可以构建出一个在功能上类似于lpop或rpop，但是却能够一次返回并弹出多个元素的操作。本章稍后将会介绍【原子地】执行多个命令的方法，而更高级的Redis事务特性则会在下一章介绍。

有几个列表命令可以将元素从一个列表移动到另一个列表，或者阻塞【block】执行命令的客户端直到有其他客户端给列表添加元素为止，这些命令之前都没有介绍过，下表列出了这些阻塞弹出命令和元素移动命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| blpop | blpop key-name \[key-name ...\] timeout |  |
| brpop | brpop key-name \[key-name ...\] timeout |  |
| rpoplpush | rpoplpush source-key dest-key |  |
| brpoplpush | brpoplpush source-key dest-key timeout |  |



