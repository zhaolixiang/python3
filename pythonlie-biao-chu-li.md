# Python列表处理

### 0、切片操作。同字符串切片操作，这里不再赘述

### 1、获取列表长度：len

实例：

```
list=["my","name","is","mark","age",18]
print(len(list))
list2=[]
print(len(list2))
```

控制台打印结果：

```
6
0
```

### 2、列表的循环遍历

* ##### for循环

```
list=["my","name","is","mark","age",18]
for item in list:
    print(item)
```

打印结果：

```
my
name
is
mark
age
18
```

* ##### while循环

```
list=["my","name","is","mark","age",18]
i=0
while i<len(list):
    print(list[i])
    i+=1
```

打印结果：

```
my
name
is
mark
age
18
```

### 3、添加元素：append、extend、insert

* ##### append：向列表添加元素，添加到尾部

实例：

```
list=["my","name","is","mark","age",18]
print("添加前：",list)
list.append("test")
print("添加后：",list)
```

打印结果：

```
添加前： ['my', 'name', 'is', 'mark', 'age', 18]
添加后： ['my', 'name', 'is', 'mark', 'age', 18, 'test']
```

* ##### extend：将另外一个列表的元素逐一添加到指定列表中

实例：

```
list=["my","name","is","mark","age",18]
print("extend前：",list)
list2=["A","B"]
list.extend(list2)
print("extend后：",list)
```

打印结果：

```
extend前： ['my', 'name', 'is', 'mark', 'age', 18]
extend后： ['my', 'name', 'is', 'mark', 'age', 18, 'A', 'B']
```

* ##### inset\(index,objectA\)：在指定位置index前面插入对象objectA

实例：

```
list=["my","name","is","mark","age",18]
print("insert前：",list)
list.insert(3,"test")
print("insert后：",list)
```

打印结果：

```
insert前： ['my', 'name', 'is', 'mark', 'age', 18]
insert后： ['my', 'name', 'is', 'test', 'mark', 'age', 18]
```

### 4、修改元素：通过下标修改指定位子元素

实例：

```
list=["my","name","is","mark","age",18]
print("修改前：",list)
list[len(list)-1]=19
print("修改后：",list)
```

结果：

```
修改前： ['my', 'name', 'is', 'mark', 'age', 18]
修改后： ['my', 'name', 'is', 'mark', 'age', 19]
```

### 5、查找元素：in、not in、index、count

* ##### in、not in查找指定元素是否存在，或者不存在

实例：

```
list=["my","name","is","mark","age",18]
print("mark" in list)
print("Mark" in list)
print("mark" not in list)
print("Mark" not in list)
```

结果：

```
True
False
False
True
```

* ##### index:查找指定元素是否存在，存在返回下标，不存在返回-1

> ##### 语法：list.index\(目标对象\[,开始下标,结束下标\]\)

实例：

```
list=["my","name","is","mark","age",18]
print(list.index("name"))
print(list.index("name",0,2))
print(list.index("name",1,3))
```

结果：

```
1
1
1
```

* ### count:返回指定对象在列表中出现的次数

实例：

```
list=["my","name","is",18,"mark","age",18]
print(list.count(18))
print(list.count("mark"))
print(list.count(19))
```

结果：

```
2
1
0
```

### 6、删除元素：del、pop、remove

> del：根据下标删除
>
> pop：删除最后一个元素，相当于弹出栈顶元素，如果指定下标，也可以删除任意位置元素。
>
> remove：根据元素的值进行删除，只删除最先找到的那个

实例：

```
list=["my","name",18,"is",18,"mark","age",18]
print("删除前：",list)


del list[1]
print("del后：",list)


list=["my","name",18,"is",18,"mark","age",18]
list.pop()
print("pop后：",list)

list=["my","name",18,"is",18,"mark","age",18]
list.pop(0)
print("pop(0)后：",list)

list=["my","name",18,"is",18,"mark","age",18]
list.remove(18)
print("remove后：",list)
```

结果：

```
删除前： ['my', 'name', 18, 'is', 18, 'mark', 'age', 18]
del后： ['my', 18, 'is', 18, 'mark', 'age', 18]
pop后： ['my', 'name', 18, 'is', 18, 'mark', 'age']
pop(0)后： ['name', 18, 'is', 18, 'mark', 'age', 18]
remove后： ['my', 'name', 'is', 18, 'mark', 'age', 18]
```

### 7、排序：sort、reverse、sorted

> sort：将数组从小到大排序，参数reverse=True可改成从大到小排序，永久排序
>
> reverse：将数组倒置，永久排序
>
> sorted：效果同sort，只不过是临时排序

实例：

```
list=[1,3,5,2,7,8,4,0]
print("排序前：",list)

list.sort()
print("sort后：",list)

list=[1,3,5,2,7,8,4,0]
list.sort(reverse=True)
print("sort(reverse=True)后：",list)


list=[1,3,5,2,7,8,4,0]
list.reverse()
print("reverse后：",list)


list=[1,3,5,2,7,8,4,0]
sorted(list,reverse=True)
print("sorted后（临时操作不影响原有列表）：",list)

list=[1,3,5,2,7,8,4,0]
print("sorted后：",sorted(list,reverse=True))
```

结果：

```
*****列表操作前：*****
1
3
5
2
7
8
4
0
*****列表sort后：*****
0
1
2
3
4
5
7
8
*****列表sort同时reverse=True后：*****
8
7
5
4
3
2
1
0
*****列表reverse后：*****
0
4
8
7
2
5
3
1
*****列表sorted后（临时操作不影响原有列表）：*****
1
3
5
2
7
8
4
0
*****列表sorted后：*****
8
7
5
4
3
2
1
0
```

### 8、列表最大值、最小值、总和：min、max、sum

实例：

```
list=[1,3,5,2,7,8,4,0]
print("列表最小值：%d"%min(list))
print("列表最大值：%d"%max(list))
print("列表总和：%d"%sum(list))
```

结果：

```
列表最小值：0
列表最大值：8
列表总和：30
```



