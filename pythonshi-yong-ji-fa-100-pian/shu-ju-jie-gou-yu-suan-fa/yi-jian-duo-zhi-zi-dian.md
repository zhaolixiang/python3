# 1、需求🙀

> 我们想要一个能将键（key）映射到多个值的字（即所谓的一键多值字典）

# 2、解决方案😸

字典是一种关联容器，每个键都映射到一个单独的值上。如果想让键映射到多个值，需要将这些多个值保存到另一个容器（列表或者集合）中。

可以这样创建字典：

```
d={
‘a’:[1,2,3],
'b':[4,5]
}
```

或者这样创建：

```
d={
'a':{1,2,3},
'b':{4,5}
}
```

要使用列表还是集合完全取决应用的意图。如果希望保留元素插入的顺序，就用列表，如果希望消除重复元素（并且不在意他们的排序），就用集合。

为了能方便的创建这样的字典，可以利用collections模块中的defaultdict类。defaultdict的一个特点就是它会自动初始化第一个值，这样只需关注添加元素即可：

```
from collections import defaultdict

d=defaultdict(list)
d['a'].append(1)
d['a'].append(2)
d['b'].append(4)

print(d)


d=defaultdict(set)
d['a'].add(1)
d['a'].add(2)
d['b'].add(4)

print(d)
```

结果：

```
defaultdict(<class 'list'>, {'a': [1, 2], 'b': [4]})
defaultdict(<class 'set'>, {'a': {1, 2}, 'b': {4}})
```



