直接看实例吧，控制多线程的执行顺序：

代码：

```
#控制多线程的执行顺序
from threading import Thread,Lock
import time

class Task1(Thread):
    def run(self):
        while True:
             if lock1.acquire():
                print("---Task1---")
                time.sleep(0.5)
                lock2.release()
class Task2(Thread):
    def run(self):
        while True:
             if lock2.acquire():
                print("---Task2---")
                time.sleep(0.5)
                lock3.release()
class Task3(Thread):
    def run(self):
        while True:
             if lock3.acquire():
                print("---Task3---")
                time.sleep(0.5)
                lock1.release()
#创建lock实例1，第一个开始不上锁
lock1=Lock()
lock2=Lock()
lock2.acquire()
lock3=Lock()
lock3.acquire()

t1=Task1()
t2=Task2()
t3=Task3()

t1.start()
t2.start()
t3.start()
```

结果：

```
---Task1---
---Task2---
---Task3---
---Task1---
---Task2---
---Task3---
---Task1---
---Task2---
---Task3---
---Task1---
---Task2---
---Task3---
...
```



