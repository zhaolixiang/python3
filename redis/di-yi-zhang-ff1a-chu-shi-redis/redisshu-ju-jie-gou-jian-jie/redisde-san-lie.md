### 散列

Redis的散列可以存储多个键值对之间的映射。和字符串一样，散列存储的值既可以是字符串也可以是数字值，并且同样可以对散列存储的数字值执行自增操作或者自减操作。

下表列出了散列的命令：

| 命令 | 行为 |
| :--- | :--- |
| hset | 在散列里面关联给定的键值对 |
| hget | 获取指定散列键的值 |
| hgetall | 获取散列包含的所有键值对 |
| hdel | 如果给定键存在于散列中，那么移除这个键 |

##### 实例：

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)

r.delete("hash-key")

print(r.hset("hash-key","sub-key1","value1")) #如果返回1，表示散列中不存在该键值对，返回0表示存在该键值对
print(r.hset("hash-key","sub-key2","value2")) #如果返回1，表示散列中不存在该键值对，返回0表示存在该键值对
print(r.hset("hash-key","sub-key1","value1")) #如果返回1，表示散列中不存在该键值对，返回0表示存在该键值对

print(r.hgetall("hash-key"))


print(r.hdel("hash-key","sub-key1")) #如果存在，就删除返回1，不存在返回0
print(r.hdel("hash-key","sub-key3")) #如果存在，就删除返回1，不存在返回0

print(r.hget("hash-key","sub-key2"))

print(r.hgetall("hash-key"))

```

结果：

```
1
1
0
{b'sub-key1': b'value1', b'sub-key2': b'value2'}
1
0
b'value2'
{b'sub-key2': b'value2'}
```



