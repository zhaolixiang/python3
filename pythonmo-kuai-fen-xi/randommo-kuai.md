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
import random

for i in range(20):
    print(random.randint(0,10),end=' ')
```

结果：

```
1 1 7 5 10 1 4 1 0 8 7 7 2 10 6 8 6 0 3 1
```

# 2、random.randrange\(start,stop,step\)

也是一个随机整数函数，参数可选

* 只有一个参数时，默认随机范围是0到该参数，前闭后开；
* 两个参数时，表示最小值和最大值，前闭后开
* 三个参数时，表示最小值，最大值和步长，前闭后开

函数算法时间复杂度：O\(1\)

核心源代码：

```
return istart+istep*self._randbelow(n) #调用randbelow函数处理
```

实例：

```
import random

for i in range(10):
    print(random.randrange(10),end=' ') #产生0到10（不包括10）的随机数

print("")

for i in range(10):
    print(random.randrange(5,10),end=' ') #产生5到10（不包括10）的随机数

print("")

for i in range(10):
    print(random.randrange(5,100,5),end=' ') #产生5到100（不包括100）范围内的5倍整数的随机数
```

结果：

```
1 1 2 4 4 3 4 6 1 4 
6 6 5 7 8 9 6 6 6 5 
30 50 20 40 75 85 25 65 80 95
```

# 3、random.choice\(seq\)

一个随机选择函数，seq是一个非空的集合，在集合中随机选择了一个元素输出，元素的类型没有限制。

核心源代码：

```
i=self._randbelow(len(seq)) #由randbelow函数得到随机地下标
return seq[i]
```

函数算法时间负责度：O\(1\)

实例：

```

```

代码：

```

```

