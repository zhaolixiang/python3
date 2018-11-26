# 1、需求🙀

> 我们需要调用一个换算函数（例如sum\(\)、min\(\)、max\(\)），但首先得对数据做转换或者筛选

# 2、解决方案😸

有一种非常优雅地方案能够将数据换算和转换结合在一起：在函数参数中使用生成器表达式。

### 实例1：计算平方和：

```
nums=[1,2,3]
s=sum(x*x for x in nums)
print(s)
```

结果：

```
14
```

### 实例2：判断指定目录是否存在.py文件

```
import os
filename=os.path.dirname(os.path.abspath(__file__))
files1=os.listdir(filename+"/image")
files2=os.listdir(filename)
#any表示该iterable只要存在一个满足条件的，欧返回True，否则才返回False
if any(name.endswith('.py') for name in files1):
    print('存在py文件')
else:
    print('不存在py文件')



#any表示该iterable只要存在一个满足条件的，欧返回True，否则才返回False
if any(name.endswith('.py') for name in files2):
    print('存在py文件')
else:
    print('不存在py文件')
```

运行结果：

```
不存在py文件
存在py文件
```

### 实例3：根据字典某个key取最小值

```
marks=[
    {'age':18,'money':100},
    {'age':19,'money':500},
    {'age':17,'money':900},
    {'age':20,'money':1000},
]
min_mark=min(m['age'] for m in marks)
print(min_mark)
```

结果：

```
17
```

# 3、分析😈

这种解决方案展示了当把生成器表达式作为函数的单独参数时在语法上的一些微妙之处（即：不必重复使用符号）。例如下面两行代码表示的是同一个意思：

```
s=sum((x*x for x in nums))
s=sum(x*x for x in nums)
```

比起首先创建一个临时例表，使用生成器做参数通常是更为高效和优雅地方式。例如，如果不使用生成器表达式，可以考虑下面方法来计算平法和：

```
nums=[1,2,3]
s=sum([x*x for x in nums])
print(s)
```

这也能工作，但这引入了一个额外的步骤并且创建了额外的列表。对于小的一个列表。是无关紧要，但是如果nums非常巨大，那么就会创建一个庞大的临时数据结构，而且只用一次就被丢弃。

基于生成器的解决方案可以以迭代的方式转换数据，因此在内存使用上要高效得多。

某些特定的换算函数比如：min\(\)和max\(\)都可以接受一个key参数，当可能倾向于使用生成器时会很有帮助。例如在上面实例【根据字典某个key取最小值】时可以考虑下面的替代方案：

```
min_mark=min(marks,key=lambda m:m['age'])
```



