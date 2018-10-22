# 1、需求🙀

> 有一系列的字典或对象实例，我们想根据某个特定的字段来分组迭代数据。

# 2、解决方案😸

itertools.groupby\(\)函数在对数据进行分组时特别有用。

实例：

```
from operator import itemgetter
from itertools import groupby

rows=[
    {'name':'mark','age':18,'uid':'110'},
    {'name':'miaomiao','age':28,'uid':'160'},
    {'name':'miaomiao2','age':28,'uid':'150'},
    {'name':'xiaohei','age':38,'uid':'130'},
]

#首先根据age排序
rows.sort(key=itemgetter('age'))

for age,items in groupby(rows,key=itemgetter('age')):
    print(age)
    for i in items:
```

结果：

```
18
{'name': 'mark', 'age': 18, 'uid': '110'}
28
{'name': 'miaomiao', 'age': 28, 'uid': '160'}
{'name': 'miaomiao2', 'age': 28, 'uid': '150'}
38
{'name': 'xiaohei', 'age': 38, 'uid': '130'}
```

# 3、分析

> 一键多值字典：defaultdict

函数groupby\(\)通过扫描序列找出拥有相同值（或是由参数key指定的函数所返回的值）的序列项，并将它们分组。groupby\(\)创建了一个迭代器，而在每次迭代时都会返回一个值（value）和一个子迭代器（sub\_iterator），这个迭代器可以产生所有在该分组内具有该值得项。

在这里重要的是首先要根据age对数据进行排序。因为groupby\(\)不会排序。

如果只是简单的根据日期将数据分组到一起，放进一个大的数据结构中以允许进行随机访问，那么利用defaultdict\(\)构建一个一键多值字典可能会更好：

```

```

结果：

```

```

不考虑排序的话，defaultdict方法一般比groupby快。

