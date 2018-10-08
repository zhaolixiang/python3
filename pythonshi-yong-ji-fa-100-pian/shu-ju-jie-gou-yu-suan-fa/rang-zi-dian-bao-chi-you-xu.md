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

> OrderedDict内部维护了一个双向链表，它会根据元素加入的顺序来排列键的位置。第一个新加入的元素被放置在链表的末尾，接下来对已存在的键做重新赋值，不会改变键的位置。
>
> 请注意：OrderedDict的大小是普通字典的2倍。这是由于它额外创建的链表所致。因此，如果打算构建一个涉及大量OrderedDict实例的数据结构（例如从CSV文件中读取100 000行行内容到OrderedDict列表中），那么需要认证对应用做需求分析，从而判断使用OrderedDict所带来的好处是否能超越因额外的内存开销所带来的缺点。



