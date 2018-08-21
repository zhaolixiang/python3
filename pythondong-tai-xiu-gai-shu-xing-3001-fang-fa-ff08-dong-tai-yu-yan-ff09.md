# Python是动态语言：动态编辑属性、动态编辑方法

### 1、运行过程中给对象绑定、删除属性

实例：

```
class Person():
    def __init__(self,name):
        self.name=name
#定义一个对象
mark=Person("mark")#终于修炼成人了，好开森
print("报出我的大名",mark.name)

#直接新定义属性：age
mark.age=18
print("我的实际年龄：",mark.age)#哎😔，写大了，我还是个宝宝👶

#动态删除属性：
del mark.age
#这种方法删除也可以
#delattr(Person,"age")
#下面一句会报错
#print("我的实际年龄：",mark.age)#哎😔，写大了，我还是个宝宝👶
```

结果：

```
报出我的大名 mark
我的实际年龄： 18
```

### 2、运行过程动态绑定方法、删除方法

```
import types
class Person():
    def __init__(self,name):
        self.name=name
    def study(self):
        print("学习使我快乐")

#定义一个方法
def run(self,speed):
    print("我竟然会跑🏃,速度：",speed)

#定义一个类方法
@classmethod
def classmethod_fun(cls):
    print("我是类方法")




#定义一个静态方法
@staticmethod
def static_fun():
    print("我是静态方法")

#定义一个对象
mark=Person("mark")
mark.study()

#动态生命函数
mark.run=types.MethodType(run,mark)
#跑起来🏃
mark.run(100)

#绑定类方法
Person.classmethod_fun=classmethod_fun
#调用类方法
Person.classmethod_fun()
mark.classmethod_fun()

#绑定静态方法
Person.static_fun=static_fun
#调用静态放
Person.static_fun()

#删除方法
del Person.classmethod_fun
#下面一句会异常
#mark.classmethod_fun()

#这种方法删除也可以
delattr(Person,"static_fun")
#下面一句会异常
#Person.static_fun()
```

运行结果：

```
学习使我快乐
我竟然会跑🏃,速度： 100
我是类方法
我是类方法
我是静态方法
```

### 3、\_\__slots_\_\_限制实例动态添加属性

> 仅仅对当前类有效，对继承类无效

实例：

```
class Person():
    __slots__ = ("name","age")
mark=Person()
mark.name="mark"
mark.age=18
#下面一句会报错
mark.object="在哪"
```

结果：

```
Traceback (most recent call last):
  File "/Users/zhaolixiang/Desktop/python/test1/python是动态语言/3、slots.py", line 8, in <module>
    mark.object="在哪"
AttributeError: 'Person' object has no attribute 'object'
```



