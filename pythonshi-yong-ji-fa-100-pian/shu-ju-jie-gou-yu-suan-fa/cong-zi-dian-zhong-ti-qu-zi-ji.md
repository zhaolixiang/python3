# 1、需求🙀

> 我们想创建一个字典，其本身是另一个字典的子集。

# 2、解决方案😸

利用字典推导式可轻松解决。

```
prices={
    'a':1.1,
    'b':2.2,
    'c':3.3,
    'd':4.4,
    'e':5.5
}
p1={key:value for key ,value in prices.items() if value>3}
print(p1)

names={'a','b'}
p2={key:value for key,value in prices.items() if key in names}
print(p2)
```

结果：

```
{'c': 3.3, 'd': 4.4, 'e': 5.5}
{'a': 1.1, 'b': 2.2}
```

# 3、分析😈

大部分可以用字典推导式解决的问题也可以通过创建元组序列然后将它们传给dict\(\)函数来完成，例如：

```
p3=dict((key,value) for key,value in prices.items() if value>3)
```

但在字典推导式的方案更加清晰，而且实际运行起来也快很多。

有时候会有多种方法来完成同一件时间。例如，第二个例子还可以重写成：

```

```

