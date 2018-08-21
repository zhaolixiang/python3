# Python==与is对比

> 本节内容很少，为啥要单拿一篇来讲呢，因为很容易理解误区。
>
> is用来比较是否引用同一个对象
>
> ==表示两个对象是否相等

实例：

```
import copy
a=[1,2,3]
b=a
c=copy.copy(a)

print("a的id：",id(a))
print("b的id：",id(b))
print("c的id：",id(c))

print("a==b",a==b)
print("a is b",a is b)
print("a==c",a==c)
print("a is c",a is c)
```

结果：

```
a的id： 4421593864
b的id： 4421593864
c的id： 4421592584
a==b True
a is b True
a==c True
a is c False
```



