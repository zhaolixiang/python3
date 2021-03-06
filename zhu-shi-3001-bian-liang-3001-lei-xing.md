# 1、注释

* 单行注释，使用\#，\#号后面的都是注射，例如

```
#我是单行注释
print("Hello Python world")
```

* 多行注释：开始和结束用三个单引号扩起来

```
'''
我是多行注释
我是多行注释
我是多行注释
'''
print("Hello Python world")
```

* 多行注释：开始和结束用三个双引号扩起来

```
"""
我是多行注释
我是多行注释
我是多行注释
"""
print("Hello Python world")
```

> 注意：单引号和双引号混合注释是不可以的呦，只能是开始结束三个单引号或者三个双引号，而不能是开始（结束）三个单引号+结束（开始）三个双引号。

# 2、变量

变量是用来存储【值】的。python中的变量【不】需要声明类型。

![](/assets/Jietu20180809-220119.jpg)

打印结果：

![](/assets/Jietu20180809-220130.jpg)

# 3、数据类型

虽然python声明变量时，不需要直接定义类型，但实际数据还是分类型处理的，下面就重要数据类型进行整理。

* ### 字符串类型

字符串类型用于指定一个字符序列，其定义方法是把文本放入单引号或者双引号或者三引号中，这三种引号形式在语义尚没有差别，但要求开始和结束使用的引号类型必须相同。

另外，单引号和双引号包含的字符串必须在同一行，而三引号的字符串可以分布在多行，并且三引号内，可以将所有格式符号（例如换行符、制表符、空格等）包含在内。

在字符串类型中，反斜杠\(\\)字符用于转移特殊字符，如换行符、反斜杠本身、引号、和非打印字符。

下表列出了可识别的一些转义码，无法识别的转义序列将保持原样，包括最前面的反斜杠在内。

| 字符 | 描述 |
| :--- | :--- |
| \ | 续行符 |
| \ | 反斜杠 |
| \' | 单引号 |
| \" | 双引号 |
| \a | Bell（音箱付出提示音） |
| \b | 退格符 |
| \e | Escape |
| \0 | Null（空值） |
| \n | 换行符 |
| \v | 垂直制表符 |
| \t | 水平制表符 |
| \r | 回车符 |
| \f | 换页符 |
| \ooo | 八进制（\000~\377） |
| \uxxxx | Unicode字符（\u0000~\uffff） |
| \Uxxxxxxxx | Unicode字符（\u00000000~\uffffffff） |
| \N（字符名称） | Unicode字符串名称 |
| \xhh | 十六进制值（x00~xff） |

转义码\ooo和\x用于将字符嵌入到很难输入的字符串字面中，例如：控制码、分打印字符、符号、国际字符等。

* ### 数字

内置的数字类型分为四种类型：布尔值、整数、浮点数、复数

##### 布尔值

标识符True和False会被解释为布尔值，其整数值分为1和0

##### 整数

像12345这个得数字被解释为十进制整数，要使用八进制、十六进制或二进制指定整数。可以在值得前面加上0o、0x或0b，例如：0o644、0x100fea8、0b11101010.

在Python中，整数的位数是任意的，所以，如果要指定一个非常大的整数，只需要写出所有位数，例如：12345678901234567890.

##### 浮点数

像123.45或1.2334e+0.2这也得数字会被解释为浮点数。

##### 复数

整数或浮点数后面加上j或J久构成了虚数，例如12.34J。

用一个实数加上一个虚数就构成了复数，方法是将实数和虚数加起来，例如：1.2+12.34J

* ### 列表

> List（列表）是python中使用最频繁的数据类型，用\[\]表述,是有序集合，后面会有专题介绍。

```
#我是列表
list=[521,'mark']
```

* ### 元祖

> 类似于列表，用\(\)标书，不能进行二次赋值，可以看做是一个只读列表

```
#我是元组
tuple=(521,'mark')
```

* ### 字典

> 通过键来取值，是无序的

```
#我是字典
dictionary={'name':'mark','age':18}
```

* ### 集合

> 无序不重复集元素集

```
set={18,"mark"}
```



