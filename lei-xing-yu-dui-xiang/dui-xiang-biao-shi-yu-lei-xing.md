内置函数id\(\)可返回一个对象的标识，返回值为整数。这个整数通常对应于该对象在内存中的位置。

is运算符用于比较两个对象的标识，依次判断两个对象是否是同一个。（==用来判断值是否相等）

内置函数type\(\)则返回该对象的类型。

对象的类型本身也是一个对象，称为对象的类，并且该对象的类时唯一的，所以可以用type\(\)查询类型，然后用is来判断是否是指定类型。

实例：

```
mark=[]
if type(mark) is list:
    print("mark is list")
```

结果：

```
mark is list
```

上面虽然可以检查类型，但坚检查最佳的方式是内置函数：isinstance\(objecy,type\):

实例：

```
mark=[]
if isinstance(mark,list):
    print("mark is list")
```

结果：

```
/Users/zhaolixiang/Desktop/python/test1/venv/bin/python /Users/zhaolixiang/Desktop/python/test1/Python参考手册/类型于对象/2、isinstance.py
mark is list
```


