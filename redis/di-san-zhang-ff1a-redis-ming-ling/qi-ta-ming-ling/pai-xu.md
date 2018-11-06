Redis的排序操作和其他编程语言的排序操作一样，都可以根据某种比较规则对一系列元素进行有序排列。负责执行排序操作的sort命令可以根据字符串、列表、集合、有序集合、散列这5种键Kim存储的数据，对列表、集合以及有序集合进行排序。如果读者之前曾经使用过关系数据库的话，那么可以将soft命令看做是sql语言里的order by。

下表展示了sort命令的定义：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| soft | soft source-key  \[by pattern\]  \[limit offset count\] \[get pattern \[get pattern ...\]\] \[asc\|desc\] \[alpha\] \[store dest-key\] | 根据给定的选项，对输入列表、集合或者有序集合进行排序，然后返回或者存储排序的结果。 |

使用sort命令提供的选项可以实现以下功能：

* 根据降序而不是默认的升序来排列元素；
* 将元素看作是数字来进行排序，或者将元素看作是二进制字符串来进行排序（比如排序字符串'110'和'12'的排序结果就跟排序数字110和12的结果不一样）；
* 使用被排序元素之外的其他值作为权重进行排序，甚至还可以从输入的列表、集合、有序集合以外的其他地方进行取值。

### 实例

```
import redis  # 导入redis包包


# 与本地redis进行链接，地址为：localhost，端口号为6379
r = redis.StrictRedis(host='localhost', port=6379)
r.delete('sort-input')

#首先将一些元素添加到列表里面
print(r.rpush('sort-input',23,15,110,7))
#根据数字大小对元素进行排序
print(r.sort('sort-input'))
#根据字母顺序对元素进行排序
print(r.sort('sort-input',alpha=True))

#添加一些用于执行排序操作和获取操作的附加数据
print(r.hset('d-7','field',5))
print(r.hset('d-15','field',1))
print(r.hset('d-23','field',9))
print(r.hset('d-110','field',3))

#将散列的域（field）用作权重，对sort-input列表进行排序
print(r.sort('sort-input',by='d-*->field'))

#获取外部数据，并将它们用作命令的返回值，而不是返回被排序的数据
print(r.sort('sort-input',by='d-*->field',get='d-*->field'))
```

运行结果：

```
4
[b'7', b'15', b'23', b'110']
[b'110', b'15', b'23', b'7']
0
0
0
0
[b'15', b'110', b'7', b'23']
[b'1', b'3', b'5', b'9']
```

> 最开头的几行代码设置了一些初始数据，然后对这些数据进行了数值排序和字符串排序，最后的代码演示了如果通过sort命令的特殊语法来将散列存储的数据作为权重进行排序，以及怎样获取并返回散列存储的数据。



