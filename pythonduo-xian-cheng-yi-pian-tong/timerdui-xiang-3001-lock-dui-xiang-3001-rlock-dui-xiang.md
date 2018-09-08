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

> 原始锁（互斥锁）是一个同步原语，状态有两种：『已锁定』、『未锁定』。
>
> 如果状态已经锁定，尝试获取锁将阻塞，直到锁被释放为止。如果有多个线程等待获取锁，当锁被释放时，只有一个线程获得它，获取顺序是不定的。

语法：

```
lock=Lock()
创建新的Lock对象，初始状态为未锁定。
```

常用方法：

```
lock.acquire(blocking):获取锁，如果有必要，需要阻塞到释放锁为止。
如果blocking为false，当无法获取锁时将立即返回False，如果成功获取锁则返回True。

lock.release():

```

### Rlock对象



