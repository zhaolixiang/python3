# Python类的定义与操作

### 1、定义类、创建对象、\_\_init\_\_

实例：

```
class People():
    """定义一个People类"""

    #__init__方法是类创建对象时默认运行的函数，用来进行初始化操作,不需要手动调用
    def __init__(self,name,age):
        """初始化属性"""
        self.name=name
        self.age=age

    def run(self):
        print("%s,快跑，帅到被人砍"%self.name)

#定义对象
mark=People("mark",18)
mark.run()

#返回对象在内存中的地址
print(mark)
```

结果：

```
mark,快跑，帅到被人砍
<__main__.People object at 0x103b5c1d0>
```

### 2、\_\__str_\_\_\(\):定义类的描述

> 如果直接print\(对象\)，会直接返回该对象在内存中的地址，现在我们想要不直接返回这个地址呢？

实例：

```
class People():
    """定义一个People类"""

    #__init__方法是类创建对象时默认运行的函数，用来进行初始化操作,不需要手动调用
    def __init__(self,name,age):
        """初始化属性"""
        self.name=name
        self.age=age

    def run(self):
        print("%s,快跑，帅到被人砍"%self.name)

    def __str__(self):
        msg="我是一个类的介绍，我的作者是个大帅哥🤓️"
        return msg

#定义对象
mark=People("mark",18)
mark.run()

#返回对象在内存中的地址
print(mark)
```

结果：

```
mark,快跑，帅到被人砍
我是一个类的介绍，我的作者是个大帅哥🤓️
```

### 3、私有属性、方法

> 只需要在属性变量名或方法名前加上\_\_就表示是私有的了，简单吧😄

实例

```
class People():
    """定义一个People类"""

    #__init__方法是类创建对象时默认运行的函数，用来进行初始化操作,不需要手动调用
    def __init__(self,name,age):
        """初始化属性"""
        #私有属性name
        self.__name=name
        self.age=age

    def run(self):
        self.__test()

    def __test(self):
        print("%s,快跑，帅到被人砍" % self.__name)

    def __str__(self):
        msg="我是一个类的介绍，我的作者是个大帅哥🤓️"
        return msg

#定义对象
mark=People("mark",18)
mark.run()

#返回对象在内存中的地址
print(mark)
```

结果：

```
mark,快跑，帅到被人砍
我是一个类的介绍，我的作者是个大帅哥🤓️
```

### 4、\_\__del_\_\_\(\):析构函数

> 对象被销毁时调用

实例：

```
class People():
    """定义一个People类"""

    #__init__方法是类创建对象时默认运行的函数，用来进行初始化操作,不需要手动调用
    def __init__(self,name,age):
        """初始化属性"""
        #私有属性name
        self.__name=name
        self.age=age

    def run(self):
        self.__test()

    def __test(self):
        print("%s,快跑，帅到被人砍" % self.__name)

    def __str__(self):
        msg="我是一个类的介绍，我的作者是个大帅哥🤓️"
        return msg

    def __del__(self):
        print("%s因为太帅了，要被干掉了"%self.__name)

#定义对象
mark=People("mark",18)
mark.run()

#返回对象在内存中的地址
print(mark)

#删除销毁对象
del mark
```

结果：

```
mark,快跑，帅到被人砍
我是一个类的介绍，我的作者是个大帅哥🤓️
mark因为太帅了，要被干掉了
```

### 5、继承

> 私有方法和属性不会被继承

* ##### 单继承

实例：

```
class Animal():
    def __init__(self,name,age):
        self.__name=name
        self.age=age

class Dog(Animal):
    def __init__(self,name,age):
        #调用父类方法
        super().__init__(name,age)
    def getAge(self):
        print("小狗年龄：",self.age)

dog=Dog(name="xDog",age=5)
dog.getAge()
```

结果：

```
小狗年龄： 5
```

* ##### 多继承

实例：

```
#python默认所有类都继承object，可写可不写
class Base(object):
    def test(self):
        print("Base--test")

class A(Base):
    def test(self):
        print("A--test")

class B(Base):
    def test(self):
        print("B--test")

class C(A,B):
    #pass表示暂时不写代码
    pass

c=C()
c.test()
```

结果：

```
A--test
```

### 6、类属性、实例属性

实例：

```
#python默认所有类都继承object，可写可不写
class Base(object):
    #共有类属性
    name="mark"
    #私有类属性
    __age=18#我的年龄是保密的哟
    pass

base=Base()
print(base.name)
print(Base.name)
#下面两句句运行错误,不能在类外访问私有属性
#print(base.__age)
#print(Base.__age)
```

结果：

```
mark
mark
```

### 7、类方法、静态方法

> 修饰器@classmethod来标识类方法，第一个参数必须是类对象
>
> 修饰符@staticmethod来标识静态方法

实例：

```
#python默认所有类都继承object，可写可不写
class Base(object):
    #共有类属性
    name="mark"
    #私有类属性
    __age=18#我的年龄是保密的哟

    #类方法
    @classmethod
    def changeAge(cls):
        cls.__age+=1
        return cls.__age

    @staticmethod
    def staticFun():
        return "类静态方法被调用"


base=Base()
#实例调用类方法
print(base.changeAge())
#类调用类方法
print(Base.changeAge())


#实例调用类静态方法
print(base.staticFun())
#类调用类静态方法
print(Base.staticFun())
```

结果：

```
19
20
类静态方法被调用
类静态方法被调用
```

### 8、\_\__new_\_\_:创建实例时调用

> 第一个参数必须是类对象。
>
> 必须要有返回值，返回的是实例化后的对象，可以return 父类的\_\__new_\_\_或者object的这个方法。

实例：

```
class Base(object):
    def __init__(self):
        print("__init__被调用了")
    def __new__(cls, *args, **kwargs):
        print("__new__被调用了")
        return super().__new__(cls)



base=Base()
```

结果：

```
__new__被调用了
__init__被调用了
```



