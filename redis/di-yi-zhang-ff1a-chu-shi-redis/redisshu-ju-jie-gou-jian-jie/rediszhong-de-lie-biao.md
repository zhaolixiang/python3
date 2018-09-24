### 列表

Redis对链表（linked-list）结构的支持使得它在键值存储的世界中独树一帜，一个列表结构可以有序的存储多个字符串。

Redis列表可执行的操作和很多编程语言里面的列表操作非常相似，下标列出了列表命令：

| 命令 | 行为 |
| :--- | :--- |
| RPUSH | 将给定值推入列表的右端 |
| LPUSH | 将给定值推入列表的左端 |
| LRANGE | 获取列表在给定范围内的所有值 |
| LINDEX | 获取列表在给定位置上的单个元素 |
| LPOP | 从列表的左端弹出一个值，并返回被弹出的值 |
| RPOP | 从列表的右端弹出一个值，并返回被弹出的值 |

##### 实例：

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)

r.delete("list-key")

print(r.rpush("list-key","item1")) #向列表推入新的元素，语句返回列表执行该语句后的长度
print(r.rpush("list-key","item2")) #向列表推入新的元素，语句返回列表执行该语句后的长度

print(r.lrange("list-key",0,-1))  #0为起始索引，-1表示列表结束索引，这样就可以取出列表的所有元素

print(r.lindex("list-key",1))  #lindex取出单个元素
```

结果

```
1
2
[b'item1', b'item2']
b'item2'
```



