> 条件变量时构建在另一个锁上的同步原语，当需要线程关注特定的状态变化或事件发生时将使用这个锁。典型的用法是生产者与消费者问题，其中一个线程生产的数据提供给另外一个线程使用。

### 语法：

```
c=Condition(lock)
穿件新的条件变量。lock时可选的Lock或RLock的实例。如果未提供lock参数，就会创建新的RLock实例供条件变量使用。
```

### 常用方法：

```
c.acquire(*args):获取底层锁。此方法将调用底层锁上对应的acquire(*args)方法。

c.release()：释放底层锁。此方法将调用底层锁上对应的release()方法

c.wait(timeout)：等待直到获取通知或出现超时为止。此方法在调用线程已经获取锁之后调用。
调用时，将释放底层锁，而且线程将进入睡眠状态，直到另一个线程在条件变量上执行notify()或notify_all()方法将其唤醒为止。
在线程被唤醒后，线程讲重新获取锁，方法也会返回。timeout是浮点数，单位为秒。
如果超时，线程将被唤醒，重新获取锁，而控制将被返回。

c.notify(n)：唤醒一个或多个等待此条件变量的线程。此方法只会在调用线程已经获取锁之后调用，
而且如果没有正在等待的线程，它就什么也不做。
n指定要唤醒的线程数量，默认为1.被唤醒的线程在它们重新获取锁之前不会从wait()调用返回。

c.notify_all()：唤醒所有等待此条件的线程。
```

### 实例：使用条件变量


