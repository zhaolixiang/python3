### 字符串

Redis的字符串和其他编程语言或者其他键值存储提供的字符串非常类似。

字符串拥有一些和其他键值存储相似的命令，例如：GET（获取值）、SET（设置值）和DEL（删除值）。

下表展示了字符串的基本命令：

| 命令 | 行为 |
| :--- | :--- |
| GET | 获取存储在给定键中的值 |
| SET | 设置存储在给定键中的值 |
| DEL | 删除存储在给定键中的值（这个命令可以用于所有类型） |

##### 实例：SET、GET、DEL使实例

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)
print(r.set("Hello","World"))#SET命令执行成功，会返回OK，在Python中返回True

print(r.get("Hello"))  #获取指定键【Hello】对应的值

r.delete("Hello")  #删除

print(r.get("Hello"))
```

结果

```
True
b'World'
None
```

 

