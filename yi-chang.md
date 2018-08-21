# Python异常

### 1、异常捕捉

实例：

```
try:
    1/0
except (ZeroDivisionError) as errorMsg:
    print("错误信息：",errorMsg)
else:
    print("没有捕捉到异常")
finally:
    print("不管有没有异常，我都会执行")
```

结果：

```
错误信息： division by zero
不管有没有异常，我都会执行
```

### 2、抛出自定义异常

> 可以用raise语句来引发异常。
>
> 自定义的异常/错误对象必须是Error或Exception类的子类

实例：

```
class MyException(Exception):
    def __init__(self,msg):
        self.msg=msg

try:
    #raise引发一个自定义异常
    raise MyException("自定义异常")
except MyException as arg:
    print(arg.msg)
else:
    print("没有捕捉到异常")
finally:
    print("不管有没有异常，我都会执行")
```

结果：

```
自定义异常
不管有没有异常，我都会执行
```



