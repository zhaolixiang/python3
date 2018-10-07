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

```



