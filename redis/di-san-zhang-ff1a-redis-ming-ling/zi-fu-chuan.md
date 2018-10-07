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

> 读者可能会发现本书只调用了incr\(\),这是因为Pythin的Redis库在内部使用incrby命令来实现incr\(\)方法，并且这个方法的第二个参数是可选的：如果用户没有为这个可选参数设置值，那么这个参数就会使用默认值1.

除了自增操作和自减操作之外，Redis还拥有对字符串的其中一部分内容进行读取或者写入的操作（这些操作也可以用于整数或者浮点数，但这种用法并不常见）。下表展示了用来处理字符串子串和二进制位的命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| append | append key-namr value | 将值value追加到给定键key-name当前存储的值得尾部 |
| getrange | getrange key-name start end | 获取一个由偏移量start至偏移量end范围内所有字符组成的子串，包括start和end在内 |
| setrange | setrange key-name offset value | 将从start偏移量开始的子串设置为给定值 |
| getbit | getbit key-name offset | 将字节串看做是二进制位串【bit string】，并返回位串中偏移量为offset的二进制位的值 |
| setbit | setbit key-name offset value | 将字节串看做是二进制位串，并将位串中偏移量为offset的二进制位设置为value |
| bitcount | bitcount key-name \[start end\] | 统计二进制位串里面值为1的二进制位的数量，如果给定了可选的start偏移量和end偏移量，那么只对偏移量范围内的二进制位进行统计 |
| bitop | bitop operation dest-key key-name \[key-namr ...\] | 对一个或多个二进制位串执行包括并【and】、或【or】、异或【xor】、非【not】在内的任意一种按位运算操作，并将计算得出的结果保存在dest-key键里面 |



