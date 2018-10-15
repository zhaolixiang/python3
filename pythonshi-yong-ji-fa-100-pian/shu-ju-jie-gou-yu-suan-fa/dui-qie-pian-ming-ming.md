# 1、需求🙀

> 我们的代码已经变得无法阅读，到处都是硬编码的切片索引，我们想优化他们。

# 2、解决方案😸

代码中如果有很多硬编码的索引值，将导致可读性和维护性都不佳。

内置的slice\(\)函数会创建一个切片对象，可以用在任何运行进行切片操作的地方。

```
items=[0,1,2,3,4,5,6]
a=slice(2,4)
print(items[2:4])
print(items[a])

items[a]=[10,11,12,13]
print(items)

del items[a]
print(items[a])
print(items)
```

运行结果：

```
[2, 3]
[2, 3]
[0, 1, 10, 11, 12, 13, 4, 5, 6]
[12, 13]
[0, 1, 12, 13, 4, 5, 6]
```

如果有一个slice对象的实例s。可以分别通过s.start、s.stop以及s.step属性得到关于该对象的信息。例如：

```
items=[0,1,2,3,4,5,6]
a=slice(2,8,3)
print(items[a])
print(a.start)
print(a.stop)
print(a.step)
```

结果：

```
[2, 5]
2
8
3
```



