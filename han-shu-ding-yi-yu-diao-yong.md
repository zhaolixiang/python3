# Python函数定义与调用

### 1、函数定义与调用

> 语法：
>
> ```
>      def 函数名():
>
>               函数代码
> ```

实例：

```
def printHello():
    print("Hello world")

#调用函数
printHello()
```

结果：

```
Hello world
```

### 2、函数的帮助文档

> help\(函数名称\)：返回对应函数的帮助文档。
>
> 在函数定义中的首行，用“”添加的就是帮助文档。

实例：

```
def printHello():
    "帮助文档：打印hello world"
    print("Hello world")

help(printHello)
```

结果：

```
Help on function printHello in module __main__:

printHello()
    帮助文档：打印hello world
```

### 3、参数与返回值

> python采用应用传参，当参数类型为不可变类型时，对参数没有影响，当参数类型为可变时，可能会修改参数

实例：

```
#定义b的默认值为3
def add(a,b=3):
    return a+b

#调用函数
print(add(1,2))
print(add(1))
```

结果：

```
3
4
```

### 4、函数嵌套调用

实例：

```
def A():
    print("A函数调用")
    def B():
        print("B函数调用了")
    print("A函数调用B函数前")
    B()
    print("函数A调用函数B后")

#调用函数
A()
```

结果：

```
A函数调用
A函数调用B函数前
B函数调用了
函数A调用函数B后
```

### 5、不定长参数

> 加了\*的参数变量，还用元组来存储多个参数。
>
> 加了\*\*的参数，会用字典来存储多个参数

实例：

```
#定义b的默认值为3
#c为元组
#d为字典
def add(a,b=3,*c,**d):
    print("a=",a)
    print("b=",b)
    print("c=",c)
    print("d=",d)

#调用函数
add(1,2)
print("*"*30)
add(1)
print("*"*30)
add(1,2,3,4,5)
print("*"*30)
add(1,2,3,4,5,name="mark",age=18)
```

结果：

```
a= 1
b= 2
c= ()
d= {}
******************************
a= 1
b= 3
c= ()
d= {}
******************************
a= 1
b= 2
c= (3, 4, 5)
d= {}
******************************
a= 1
b= 2
c= (3, 4, 5)
d= {'name': 'mark', 'age': 18}
```

### 6、匿名函数

> 用lambda关键词可以创建小型函数，省略了用def关键字来声明函数的标准步骤。

* ##### 匿名函数声明与调用

实例：

```
add=lambda a,b:a+b

print(add(2,3))
```

结果：

```
5
```

* ##### 匿名函数作为参数传递

实例：

```
def fun(a,b,lam):
    print("a=",a)
    print("b=",b)
    print("a+b=",lam(a,b))

add=lambda a,b:a+b
fun(4,5,add)
```

结果：

```
a= 4
b= 5
a+b= 9
```

* ##### 匿名函数用来协助排序

实例：

```
#简单列表排序很简单
ages=[18,19,17]
print(ages)
ages.sort()
print(ages)

#当列表内包含的是字典，怎么根据字典内的age排序呢？
infors=[
    {"name":"mark","age":18},
    {"name":"sq","age":19},
    {"name":"xman","age":17}
]
print(infors)
'''
下面匿名函数等效于：
     def  fun(x):
         return x["age"]
'''
infors.sort(key=lambda x:x["age"])
print(infors)
```

结果：

```
[18, 19, 17]
[17, 18, 19]
[{'name': 'mark', 'age': 18}, {'name': 'sq', 'age': 19}, {'name': 'xman', 'age': 17}]
[{'name': 'xman', 'age': 17}, {'name': 'mark', 'age': 18}, {'name': 'sq', 'age': 19}]
```

### 7、函数重用

> 导入整个模块：import 模块名
>
> 导入特定的函数：from 模块名 import 特定函数
>
> 使用as给函数重命名：from 模块名 import 特定函数 as 新名称
>
> 使用ad给模块冲命名 import 模块名 as  新名词
>
> 导入模块的所有函数：from 模块名 import  \*



