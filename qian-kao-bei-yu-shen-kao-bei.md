# Python浅拷贝与深拷贝

### 1、浅拷贝

> 浅拷贝：拷贝了引用，没有拷贝内容。

实例：

```
a=[1,2,3]
b=a

print("a的id：",id(a))
print("b的id：",id(b))

a.append(4)
print(a)
print(b)
```

结果：

```
a=[1,2,3]
b=a

print("a的id：",id(a))
print("b的id：",id(b))

a.append(4)
print(a)
print(b)
```

### 2、深拷贝

> 深拷贝：对于一个对象所有层次的拷贝（递归）。
>
> 使用copy.deepcopy\(\)完成深拷贝

实例：

```
import copy
a=[1,2,3]
b=[4,5,6]

c=[a,b]

d=c
e=copy.deepcopy(c)




print("c的值",c)
print("d的值",d)
print("e的值",e)

print("c的id：",id(c))
print("d的id：",id(d))
print("e的id：",id(e))

a.append(7)
print("c的值",c)
print("d的值",d)
print("e的值",e)

print("c的id：",id(c))
print("d的id：",id(d))
print("e的id：",id(e))
```

结果：

```
c的值 [[1, 2, 3], [4, 5, 6]]
d的值 [[1, 2, 3], [4, 5, 6]]
e的值 [[1, 2, 3], [4, 5, 6]]
c的id： 4329011592
d的id： 4329011592
e的id： 4333645768
c的值 [[1, 2, 3, 7], [4, 5, 6]]
d的值 [[1, 2, 3, 7], [4, 5, 6]]
e的值 [[1, 2, 3], [4, 5, 6]]
c的id： 4329011592
d的id： 4329011592
e的id： 4333645768
```

### 3、cope.cope\(\)对可变类型和不可变类型影响不同

实例：

```
import copy
a=[1,2,3]
b=copy.copy(a)

print("a的id：",id(a))
print("b的id：",id(b))

a.append(4)
print(a)
print(b)


#不可变类型
c=(1,2,3)
d=copy.copy(c)

print("c的id：",id(c))
print("d的id：",id(d))
```

结果：

```
a的id： 4372057096
b的id： 4372055816
[1, 2, 3, 4]
[1, 2, 3]
c的id： 4372006736
d的id： 4372006736
```



