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



