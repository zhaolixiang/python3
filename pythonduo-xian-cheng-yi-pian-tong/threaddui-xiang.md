# Thread对象

Thread类用于表示单独的控制线程。

### 语法：

```
t=Thread(group=None,target=None,name=None,args=(),kwargs={})
创建一个新的Thread实例：t

group：为以后扩张保留的，默认为None

target：一个可调用对象，线程启动时，run()方法将调用此对象

name：线程名称，默认创建一个“Thread-N”格式的唯一名称。

args:传递给target函数的参数元祖

kwargs:传递给target的关机字参数的字典。
```

### 常用属性于方法

```
t.start():通过在一个单独的控制线程中调用run(),启动线程，此方法只能被调用一次。

t.run()：线程启动时将调用此方法。默认情况下，他会调用目标函数target。还可以在Thread的子类中重新定义此方法。

t.join([timeout])：阻塞线程，等待直到线程终止或者出现超时为止。timeout是以秒为单位的超时时间。
线程启动之前不能调用此方法，否则会报错。

t.is_alive

t.name

t.ident

t.daemon
```



