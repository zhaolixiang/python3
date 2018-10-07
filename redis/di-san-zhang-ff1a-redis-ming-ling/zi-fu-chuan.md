在Redis里面，字符串可以存储以下三种类型的值：

* 字节串【byte string】
* 整数
* 浮点数

用户可以通过给定一个任意的数值，对存储着整数或者浮点数的字符串执行自增【increment】或者自减【decrement】操作，在有需要的时候，Redis还好将整数转换成浮点数。整数的取值范围和系统的长整数【long integer】的取值范围相同【在32位系统上，整数就是32位有符号整数，在64位系统上，整数就是64位有符号整数】，而浮点数的取值范围和精度则与IEEE754标准的双精度浮点数【double】相同。Redis明确的区分字节串、整数和浮点数的做法是一种优势，比起只能够存储字节串的做法，Redis的做法在数据表现方面具有更大的灵活性。

本节将对Redis里面最简单的结构：字符串进行讨论，介绍基本的数值自增和自减操作，以及二进制位【bit】和子串【substring】处理命令，读者可能会惊讶的发现，Redis里面最简单的机构居然也有如此强大的作用。

下表展示了对Redis字符串执行自增和自减操作的命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| incr | incr key-name | 将键存储的值+1 |
| decr | decr key-name | 将键存储的值-1 |
| incrby | incrby key-name amount | 将键存储的值加上整数amount |
| decrby | decrby key-name amount | 将键存储的值减去整数amount |
| incrbyfloat | incrbyfloat key-name amount | 将键存储的值加上浮点数amount，这个命令在Redis2.6或以上的版本可用。 |

当用户将一个值存储到Redis字符串里面的时候，如果这个值可以被解释【interpret】为十进制或者浮点数，那么Redis会察觉到这一点，并允许用户对这个字符串执行各种【incr\*】和【decr\*】操作。

如果用户对一个不存在的键或者一个保存了空串的键执行自增或者自减操作，那么Redis在执行操作时会将这个键的值当做是0来处理。

如果用户尝试对一个值无法被解释为整数或者浮点数的字符串执行自增或者自减操作，那么Redis将想用户返回一个错误。

下面代码展示了对字符串执行自增和自减操作的一些例子：

```
import redis #导入redis包包

#与本地redis进行链接，地址为：localhost，端口号为6379
r=redis.StrictRedis(host='localhost',port=6379)

r.delete('key')
#尝试获取一个不存在的键将得到一个None值。
print(r.get('key'))

#对不存在的键执行自增操作
print(r.incr('key'))
print(r.incr('key',15))

#执行自减操作
print(r.decr('key',5))

print(r.get('key'))

print(r.set('key',99))
print(r.incr('key'))
```

运行结果：

```
None
1
16
11
b'11'
True
100
```



