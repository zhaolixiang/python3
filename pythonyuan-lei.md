# Python元类

> 本篇内容只供了解，实际上99%的python程序员都用不到元类哟，不过你要想成为那1%，请随意享用，别客气😆
>
> 元类作用：拦截类的创建、修改类、返回修改之后的类

万物皆对象，类创建了实例对象，那既然万物皆对象，类也是对象，那什么创建了类呢，啦啦啦，当然就是本篇的主人翁：元类。

### 1、类也是对象

> 既然类也是一个对象，那就应该具有对象共有的特性：
>
> 可以将其赋给变量
>
> 可以拷贝
>
> 可以增加属性
>
> 可以作为函数参数进行传递

实例：

```
class A:
    def hello_fun(self):
        print("Hello World")

def test(a):
    print("类作为函数参数传递：",a)

#类也是对象，所以可以打印
print(A)

#因为类也是对象，所以可以作为函数参数进行传递
test(A)

#hasattr用来判断指定类中是有有对应属性
print(hasattr(A,"mark"))#哼，我就是一个属性，来打我呀

#添加属性并且赋值
A.mark="帅"#mark是属性名称，帅是值
print(hasattr(A,"mark"))
print("mark属性值：",A.mark)

#因为类是一个对象，所以可以拷贝
B=A#拷贝
print(B)
```

运行结果：

```
<class '__main__.A'>
类作为函数参数传递： <class '__main__.A'>
False
True
mark属性值： 帅
<class '__main__.A'>
```

### 2、动态（函数内部）创建类

> 既然类也是对象，当然可以动态创建，比如在函数内部创建。

实例：

```
def test():
    class A:
        def hello_fun(self):
            print("Hello World")
    print("函数内部创建的类：",A)


test()
```

运行结果：

```
函数内部创建的类： <class '__main__.test.<locals>.A'>
```

### 3、type动态创建类

> type函数除了可以检查类型外，另一个高大上的用途就是动态创建类。
>
> 语法：type\(类名,由父类名称组成的元组（可以为空）,包含属性的字典（名称和值）\)

实例：

```
#定义一个没有方法，没有属性的类：A
A=type("A",(),{})

#打印A
print(A)
#打印A的实例
print(A())

print("*"*30)

#定义一个带两个属性的类：B
B=type("B",(),{"name":"mark","age":18})
print(B.name)
print(B.age)

print("*"*30)

#定义一个带普通方法的类：C
def test_for_c(self):
    print(self.name)
C=type("C",(),{"name":"mark","age":18,"test_for_c":test_for_c})
c=C()
c.test_for_c()

print("*"*30)

#添加静态方法
@staticmethod
def static_method_test():
    print("static method test")
D=type("D",(),{"name":"mark","age":18,"static_method_test":static_method_test})
D.static_method_test()


print("*"*30)
#添加类方法
@classmethod
def class_method_test(cls):
    print(cls.name)
E=type("E",(),{"name":"mark","age":18,"class_method_test":class_method_test})
e=E()
e.class_method_test()
E.class_method_test()
```

结果：

```
<class '__main__.A'>
<__main__.A object at 0x10b7ac0f0>
******************************
mark
18
******************************
mark
******************************
static method test
******************************
mark
mark
```

### 4、\_\__metaclass_\_\_

实例：

```
class A(object,metaclass=xxxx):
    pass
```

滴答滴答，想象一下，解析指针现在运行到这个类的上面，此处类还没加载到内存中，然后开始解析A，首先去A的定义中找，有没有指定\_\__metaclass_\_\_属性，如果指定了，就利用这个指定的属性来创建类，没有指定就会用type开生成类。具体步骤如下：

1. 类中有这个\_\__metaclass_\_\_属性吗，如果有，利用这个属性来创建类。
2. 如果类中不存在，判断父类是否存在，如果存在，就利用这个属性创建类。
3. 父类也没有，就去模块层次寻找这个属性，存在的话就利用这个属性创建类，
4. 如果还没有找到，就使用type来创建类。

### 5、自定义元类（通过定义方法）

> 现在自定义一个元类，保证所有的属性名都大写

实例：

```
def upper_attr(class_name,class_parent,class_attr):
    #遍历属性字典，把不是__开头的属性名称都编程大写
    newAttr={}
    for name,value in class_attr.items():
        if not name.startswith("__"):
            newAttr[name.upper()]=value
    #调用type来创建类
    return type(class_name,class_parent,newAttr)
class A(object,metaclass=upper_attr):
    mark="帅哥"

print(hasattr(A,"mark"))
print(hasattr(A,"MARK"))

print(A.MARK)
```

结果：

```
False
True
帅哥
```

### 6、自定义元类（通过继承type）

> 现在自定义一个元类，保证所有的属性名都大写

实例：

```
class UpperAttrMetaClass(type):
    #__new__是在__init__之前被调用的特殊方法，用来创建对象并返回，
    #而__init__只是将传递的参数赋给对象
    def __new__(cls,class_name,class_parent,class_attr):
        # 遍历属性字典，把不是__开头的属性名称都编程大写
        newAttr = {}
        for name, value in class_attr.items():
            if not name.startswith("__"):
                newAttr[name.upper()] = value
        # 方法1：调用type来创建类
        #return type(class_name, class_parent, newAttr)

        #方法2：复用type.__new__方法
        #return type.__new__(cls,class_name,class_parent,newAttr)

        #方法3：使用super方法
        return super(UpperAttrMetaClass,cls).__new__(cls,class_name,class_parent,newAttr)



class A(object,metaclass=UpperAttrMetaClass):
    mark="帅哥"

print(hasattr(A,"mark"))
print(hasattr(A,"MARK"))

print(A.MARK)
```

结果：

```
False
True
帅哥
```



