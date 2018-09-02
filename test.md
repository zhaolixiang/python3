# 6、共享数据与同步

> 我们现在知道，进程之间彼此是孤立的，唯一通信的方式是队列或管道，但要让这两种方式完成进程间通信，底层离不开共享内容，这就是今天的主角：共享内存。

* ##### Value

```
v=Value(typecode,arg1,...,argN,lock):
typecode:要么是包含array模块使用的相同类型代码（如'i'、'd'等）的字符串，要么是来自ctypes模块的类型对象
（例如：ctypes.c_int,ctypes.c_double等）。
arg1,...,argN:传递给构造函数的参数。
lock：只能使用关键字传入的参数，默认为True：将创建一个新锁来保护对值的访问。如果传入一个现有锁，该锁将用于进行同步。

访问底层的值：v.value
```

* ##### RawValue

```
a=RawValue(typecode,arg1,...,argN):同Value对象，唯一区别是不存在lock
```

* ##### Array

```

```



