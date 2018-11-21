random是Python产生伪随机数的模块，随机种子默认为系统时钟。下面分析模块中的方法：

# 1、random.randint\(start,stop\)

这是一个产生整数随机数的函数，参数start代表最小值，参数stop代表最大值，两端的数值都可以取到；

函数算法时间复杂度为：O\(1\)

核心源码：

```
return self.randrange(a,b+1) #调用randrange函数来处理
```

实例：

```

```

