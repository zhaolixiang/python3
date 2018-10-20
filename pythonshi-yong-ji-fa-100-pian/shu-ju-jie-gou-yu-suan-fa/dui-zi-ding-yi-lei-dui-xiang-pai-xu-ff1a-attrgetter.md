# 1、需求🙀

> 对自定义的类组成的列表进行排序。

# 2、解决方案😸

内建的sorted\(\)函数可接受一个用来传递可调用对象\(callable\)的参数key，而该可调用对象会返回待排序对象中的某些值，sorted则利用这些值来比较对象。

实例：

```
from operator import attrgetter
class User:
    def __init__(self,userId):
        self.userId=userId

    def __repr__(self):
        return 'User({})'.format(self.userId)

users=[User(40),User(20),User(30)]
print(users)

#方法1
print(sorted(users,key=lambda u:u.userId))

#方法2
print(sorted(users,key=attrgetter('userId')))
```

运行结果：

```
[User(40), User(20), User(30)]
[User(20), User(30), User(40)]
[User(20), User(30), User(40)]
```



