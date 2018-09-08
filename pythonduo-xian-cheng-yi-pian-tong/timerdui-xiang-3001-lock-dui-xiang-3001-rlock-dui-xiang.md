# Timer对象、Lock对象、Rlock对象

### Timer对象

> Timer对象用于在稍后的某个时间执行一个函数。

##### 语法：

```
t=Timer(interval,func,args,kwargs)
创建定时器对象，在interval秒后运行函数func，args和kwargs提供传递给func的参数和关键字参数。
在调用start()方法后才能启动计定时器。
```

##### 常用方法：

```
t.start():启动定时器。

t.cancal()：如果函数还未执行，取消定时器。
```

### Lock对象

### Rlock对象



