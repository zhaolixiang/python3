# Python设计模式：持续更新中

### 1、单例模式

实例：

```
class Singleton(object):
    #私有类属性，存储唯一的实例对象
    __instance=None
    def __new__(cls, *args, **kwargs):
        if not cls.__instance:
            #如果没有实例化，就去实例化
            cls.__instance=super().__new__(cls)
        return cls.__instance

    def __init__(self,name):
        print("__init__方法调用了")
        self.name=name


a=Singleton("aa")
b=Singleton("bb")

print(id(a))
print(id(b))

a.name="Mark"
print(b.name)
```

结果：

```
__init__方法调用了
__init__方法调用了
4472884976
4472884976
Mark
```



