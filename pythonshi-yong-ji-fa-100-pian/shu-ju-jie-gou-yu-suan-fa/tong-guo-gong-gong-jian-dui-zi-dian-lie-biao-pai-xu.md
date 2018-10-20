# 1、需求🙀

> 我们有一个字典列表，想根据一个或多个字典中的值对列表进行排序。

# 2、解决方案😸

利用operator模块中的itemgetter函数对这类结构进行排序是非常简单的。

* ##### 实例：

```
from operator import itemgetter
rows=[
    {'name':'mark','age':18,'uid':'110'},
    {'name':'miaomiao','age':28,'uid':'150'},
    {'name':'miaomiao','age':8,'uid':'150'},
    {'name':'xiaohei','age':38,'uid':'130'},
]

rows_by_name=sorted(rows,key=itemgetter('name'))
rows_by_uid=sorted(rows,key=itemgetter('uid'))
print(rows_by_name)
print(rows_by_uid)


#itemgetter还支持多个键
rows_by_name_age=sorted(rows,key=itemgetter('name','age'))
print(rows_by_name_age)
```

运行结果：

```
[{'name': 'mark', 'age': 18, 'uid': '110'}, {'name': 'miaomiao', 'age': 28, 'uid': '150'}, {'name': 'miaomiao', 'age': 8, 'uid': '150'}, {'name': 'xiaohei', 'age': 38, 'uid': '130'}]
[{'name': 'mark', 'age': 18, 'uid': '110'}, {'name': 'xiaohei', 'age': 38, 'uid': '130'}, {'name': 'miaomiao', 'age': 28, 'uid': '150'}, {'name': 'miaomiao', 'age': 8, 'uid': '150'}]
[{'name': 'mark', 'age': 18, 'uid': '110'}, {'name': 'miaomiao', 'age': 8, 'uid': '150'}, {'name': 'miaomiao', 'age': 28, 'uid': '150'}, {'name': 'xiaohei', 'age': 38, 'uid': '130'}]
```

* ##### 讨论

在这个例子中，rows被传递给内建的sorted\(\)函数，该函数接受一个关键字参数key，这个参数应该代表一个可调用对象\(callable\)，该对象从rows中接受一个单独的元素作为输入并返回一个用来做排序依据的值。itemgetter\(\)函数创建的就是这样的一个可调用对象。

函数operator.itemgetter\(\)接受的参数可作为查询的标记，用来从rows的记录中提取出所需要的值。它可以是字典的键名称、用数字表示的列表元素或是任何可以传给对象的\_\__getitem_\_\_\(\)方法的值。如果传多个标记给itemgetter\(\)，那么它产生的可调用对象将返回一个包含所有元素在内的元组，然后sorted\(\)将根据对元组的排序结果来排列输出结果。如果想同时针对多个字段做排序，那么这是非常有用的。

有时候会用lambda表达式取代itemgetter\(\)的功能，例如：

```
rows_by_uid=sorted(rows,key=lambda r:r['uid'])
rows_by_name_age=sorted(rows,key=lambda r:(r['name','age']))
```

这种解决方案通常也能正常工作。但是用itemgetter\(\)通常会运行的更快一些。因此如果需要考虑性能问题的话，应该使用itemgetter\(\).



