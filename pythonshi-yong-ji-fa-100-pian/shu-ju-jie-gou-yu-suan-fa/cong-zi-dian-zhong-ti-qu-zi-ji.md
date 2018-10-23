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



