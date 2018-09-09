> 在一个进程内的所有线程共享全局变量。但多线程对全局变量的更改会导致变量值得混乱。

### 实例1：验证同一个进程内所有线程共享全局变量

##### 代码：

```
#验证同一个进程内的所有线程共享全局变量
from threading import  Thread
import time
g_num=1000
def work1():
    global g_num
    g_num+=3
    print("work1----num:",g_num)

def work2():
    global g_num
    print("work2---num:",g_num)

if __name__ == '__main__':
    print("start---num:",g_num)
    t1=Thread(target=work1)
    t1.start()

    #故意停顿一秒，以保证线程1执行完成
    time.sleep(1)

    t2=Thread(target=work2)
    t2.start()
```

##### 结果：

```
start---num: 1000
work1----num: 1003
work2---num: 1003
```



