协程是Tornado中进行异步I/O代码开发的方法。协程使用了Python关键字yield将调用者挂起和恢复执行。所以在学习协程之前，我们先熟悉一下yield的概念和使用方法，而要想理解yield，需要先理解迭代器的概念。

### 1、迭代器

> 迭代器（Iterator）是访问集合内元素的一种方式。迭代器对象从集合的第1个元素开始访问，直到所有元素都被访问一遍后结束。迭代器不能后退，只能前进迭代。

Python种最常用迭代器的场景是循环语句for，它用迭代器封装集合，并且煮个访问集合元素以执行循环。

例如：

```
for number in range(5):#range返回一个列表
    print(number)
```

其中的range\(\)返回一个包含所指定元素的集合，而for语句将其封装成一个迭代器后访问，使用iter\(\)可以讲列表、集合转换成迭代器，例如：

```
numbers=[1,2,3,4,5]
#t就是迭代器
t=iter(numbers)
#打印t对象，以便查看其类型
print(t)
```

返回结果：

```
<list_iterator object at 0x10e805748>
```

迭代器与普通Python对象相比，多了一个`__next__()`方法，每次调用该方法可以返回一个元素，调用者（例如for语句）可以通过不断调用`__next__()`方法来煮个访问集合元素。

例如：

```
numbers=[1,2,3,4,5]
#t就是迭代器
t=iter(numbers)
#打印t对象，以便查看其类型
print(t.__next__())
print(t.__next__())
print(t.__next__())
print(t.__next__())
```

返回结果：

```
1
2
3
4
```

调用者可以一直调用`__next__()`方法，直到返回StopIteration异常。

### 2、使用yield

> 迭代器在Python编程种的使用范围很广，那么开发者如何定制自己的迭代器呢？
>
> 答案是使用yield关键字。
>
> 调用任何定义包含yield关键字的函数都不会执行该函数，而是会获得一个队应于该函数的迭代器。

实例：

```
import time
def demoIternator():
    print("---1---")
    yield 1
    print("---2---")
    yield 2
    print("---3---")
    yield 3
    print("---4---")

for x in demoIternator():
    print(x)
    time.sleep(1)
```

结果

![](/assets/5、使用yield.gif)

