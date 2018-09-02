# 6、共享数据与同步

> 我们现在知道，进程之间彼此是孤立的，唯一通信的方式是队列或管道，但要让这两种方式完成进程间通信，底层离不开共享内容，这就是今天的主角：共享内存。

### 创建共享值得方法

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
r=RawValue(typecode,arg1,...,argN):同Value对象，唯一区别是不存在lock
```

* ##### Array

```
a=Array(typecode,initializer,lock):在共享内存中创建ctypes数组。
initializer：要么是设置数组初始大小的整数，要么是项序列，其值和大小用于初始化数组。
可以使用标准的Python索引、切片、迭代操作访问它，其中每项操作均→锁进程同步，
对于字节字符串，a还具有a.value属性，可以把整个数组当做一个字符串进行访问。
```

* ##### RawArray

```
r=RawArray(typcode,initlizer):同Array，单不存在锁。当所编写的程序必须一次性操作大量的数组项时，
如果同时使用这种数据类型和用于同步的单独大的锁，性能将极大提升。
```

* ##### 同步原语

##### 除了使用上面方法创建共享值



