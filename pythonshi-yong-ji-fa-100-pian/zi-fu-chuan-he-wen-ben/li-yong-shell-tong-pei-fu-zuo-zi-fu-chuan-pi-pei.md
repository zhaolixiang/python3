# 1、需求🙀

> 当工作在UNIX Shell下时，我们想使用常见的通配符模式（即：\*.py，Dat\[0-9\]\*.csv等）来对文本做匹配。

# 2、解决方案😸

fnmatch模块提供了两个函数：fnmatch\(\)和fnmatchcase\(\)，可用来执行这样的匹配，使用起来非常简单。

实例：

```
from fnmatch import fnmatch,fnmatchcase
print(fnmatch('mark.txt','*.txt'))
print(fnmatch('mark.txt','?ark.txt'))
print(fnmatch('mark2018.txt','?ark201[0-9].txt'))
```

运行结果：

```
True
True
True
```

一般来说，fnmatch\(\)的大小写匹配规则与底层文件相同，例如：

```
print(fnmatch('mark.txt','*.TXT'))
```

上面代码，在Max下运行为False，在Windows下运行为True。

如果这个大小写区别对我们很重要，我们就应该使用fnmatchcase\(\)。它会完全根据我们提供的大小写方法来做匹配。

实例：

```
from fnmatch import fnmatch,fnmatchcase
print(fnmatchcase('mark.txt','*.TXT'))
```

结果：

```
False
```

关于这些函数，一个常被忽略的特性是它们在处理非文件名式的字符串时的潜在用途。

例如，

```

```

可以

1

# 3、分析😈



