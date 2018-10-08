# 1、需求🙀

> 我们想创建一个字典，同时当对字典做迭代或序列化操作时，也能控制其中元素的顺序。

# 2、解决方案😸

要控制字典中元素的顺序，可以使用collections模块中的OrderedDict类。当对字典做迭代时，它会严格按照元素初始添加的顺序进行。

```
from collections import OrderedDict

d=OrderedDict()
d['a']=1
d['b']=2
d['c']=3
d['d']=4

#根据插入删除输出
for key in d:
    print(key,d[key])
```

结果：

```
a 1
b 2
c 3
d 4
```

当想构建一个映射结构以便稍后对其做序列化或编码成另一种格式时，OrderedDict就显得特别有用。例如：如果想在进行JSON编码时精确控制各字段的顺序，那么只要首先在OrderedDict中构建数据就可以了：

```
from collections import OrderedDict
import json

d=OrderedDict()
d['a']=1
d['b']=2
d['c']=3
d['d']=4

j=json.dumps(d)
print(j)


```

结果：

```
{"a": 1, "b": 2, "c": 3, "d": 4}
```



