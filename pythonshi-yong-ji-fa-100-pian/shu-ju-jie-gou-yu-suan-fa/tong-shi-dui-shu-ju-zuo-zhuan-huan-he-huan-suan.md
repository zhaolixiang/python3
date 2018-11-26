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

d

# 3、分析😈



