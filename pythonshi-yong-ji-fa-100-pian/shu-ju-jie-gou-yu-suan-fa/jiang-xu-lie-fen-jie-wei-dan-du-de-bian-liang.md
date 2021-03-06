# 1、需求🙀

> 现在有一个包含N个元素的元组或序列，现在想将它分解为N个单独的变量。

# 2、解决方案😸

在python中，任何序列、元组、可序列号对象，都可以通过一个简单的赋值操作来分解为单独的变量。

唯一要求是变量的总数和结构要和序列的相吻合。如果不吻合就会报错

实例展示：

```
#将序列分解为单独的变量
m=(1,2)
x,y=m
print("x=",x)
print("y=",y)

print("*"*30)

data=["mark",18,"超级帅",(1992,5,4)]
name,age,feature,birthday=data
print("name=",name)
print("age=",age)
print("feature=",feature)
print("birthday=",birthday)
print("*"*30)


name,age,feature,(year,mon,day)=data
print("name=",name)
print("age=",age)
print("feature=",feature)
print("year=",year)
print("mon=",mon)
print("day=",day)
```

结果

```
x= 1
y= 2
******************************
name= mark
age= 18
feature= 超级帅
birthday= (1992, 5, 4)
******************************
name= mark
age= 18
feature= 超级帅
year= 1992
mon= 5
day= 4
```

# 3、思考🤔

实际上不仅仅只是元组列表，只要对象是可迭代的，那么就可以执行分解操作，这包括字符串、文件、迭代器、生成器。

实例展示：

```
#将序列分解为单独的变量
mark="mark"
m,a,r,k=mark
print(m)
print(a)
print(r)
print(k)
print("*"*30)

#有时候我们想丢弃某个值，单由于变量数量必须和要分解的对象的可分解数量相同，此时我们可以使用_来表示要丢弃的值。

mark="mark"
m,a,r,_=mark
print(m)
print(a)
print(r)
#其实_还是一个变量，指示看起来舒服点
print(_)
```

结果：

```
m
a
r
k
******************************
m
a
r
k
```

# 4、需求升级🙀

> 假如可序列号对象可分解为N个元素，难道我们就要创建N个元素吗？如果N值非常大怎么办？

# 5、解决方案升级😸

Python中的『\*表达式』可以满足上述需求。例如，有无数个成绩列表：grades，现在想去掉第一个成绩和最后一个成绩，然后求剩下成绩的平均值：

代码

```
import numpy as np

grades=list(range(10))#定义一个0-999的分数列表
print("grades:"+str(grades))
first,*middle,last=grades
print("middle:"+str(middle))
print("去掉第一个和最后一个分数后的平均值："+str(np.mean(middle)))
```

结果

```
grades:[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
middle:[1, 2, 3, 4, 5, 6, 7, 8]
去掉第一个和最后一个分数后的平均值：4.5
```

当然这个【\*表达式】可以位于第一个位置，也可以是最后一个，或者其它位置。

假如有一些用户记录，记录由姓名和电子邮件地址组成，后面跟着任意数量的电话号码：

```
record=('mark','1782980833@qq.com','18321859453','18956245389')
name,email,*phone_numbers=record

print(name)
print(email)
print(phone_numbers)
```

运行结果：

```
mark
1782980833@qq.com
['18321859453', '18956245389']
```

# 6、\*表达式技巧

### \*表达式在迭代一个变长的元组序列时尤其有用

代码：

```
records=[
    ('foo',1,2),
    ('bar','hello'),
    ('foo',3,4),
]

def do_foo(x,y):
    print('foo',x,y)

def do_bar(s):
    print('bar',s)

for tag,*args in records:
    if tag=='foo':
        do_foo(*args)
    elif tag=='bar':
        do_bar(*args)
```

结果：

```
foo 1 2
bar hello
foo 3 4
```

### 当和某些特定的字符串处理操作（例如拆分操作）相结合时，也非常有用

代码：

```
line='nobody:*:-2:-2:unp user:/var/empty:/user/nim/false'

uname,*fileds,homedir,sh=line.split(':')
print(uname)
print(homedir)
print(sh)
```

结果：

```
nobody
/var/empty
/user/nim/false
```



