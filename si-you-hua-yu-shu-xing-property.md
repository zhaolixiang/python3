# 私有化规则与属性Property

### 1、私有化

变量定义规则

| 变量形式 | 解读 |
| :--- | :--- |
| xx | 公有变量xx |
| \_xx | 单前置下划线，私有化属性或方法，from somemodule import \*禁止导入，类对象和子类进可以访问。 |
| \_\_xx | 双前置下划线，避免与子类中的属性命名冲突，无法再在外部直接访问。 |
| \_\_xx\_\_ | 双前后下划线，python自定义使用的的属性或者方法名称，例如\_\_init\_\_，不要自定义这种名称的变量 |
| x\_ | 单后置下划线，避免与Python关键词冲突 |

### 2、属性Property

* ##### 私有属性添加getter和setter方法

实例：

```
class Man():
    def __init__(self):
        self.__age=18
    def getAge(self):
        return self.__age
    def setAge(self,value):
        #isinstance用来判断一个对象是不是指定类型
        #下面语句就是用来判断value是不是int类型
        if isinstance(value,int):
            self.__age=value
        else:
            print("输入的年龄不是整数")

if __name__=="__main__":
    mark=Man()

    mark.setAge("xxx")
    print(mark.getAge())
    mark.setAge(20)
    print(mark.getAge())

    #下面一句会报错
    #print(mark.age)
```

结果：

```
输入的年龄不是整数
18
20
```

* ##### 使用property升级getter和setter方法

实例：

```
class Man():
    def __init__(self):
        self.__age = 18

    def getAge(self):
        return self.__age

    def setAge(self, value):
        # isinstance用来判断一个对象是不是指定类型
        # 下面语句就是用来判断value是不是int类型
        if isinstance(value, int):
            self.__age = value
        else:
            print("输入的年龄不是整数")
    age=property(getAge,setAge)


if __name__ == "__main__":
    mark = Man()

    mark.age="xxx"
    print(mark.age)
    mark.age=20
    print(mark.age)
```

结果：

```
输入的年龄不是整数
18
20
```

* ##### 使用property取代getter和setter方法

实例：

```
class Man():
    def __init__(self):
        self.__age = 18

    @property
    def age(self):
        return self.__age
    @age.setter
    def age(self,value):
        # isinstance用来判断一个对象是不是指定类型
        # 下面语句就是用来判断value是不是int类型
        if isinstance(value, int):
            self.__age = value
        else:
            print("输入的年龄不是整数")


if __name__ == "__main__":
    mark = Man()

    mark.age="xxx"
    print(mark.age)
    mark.age=20
    print(mark.age)
```

结果：

```
输入的年龄不是整数
18
20
```



