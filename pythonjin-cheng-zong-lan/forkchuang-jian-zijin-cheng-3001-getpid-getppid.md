# 1、fork\(\):创建子进程、getpid\(\)、getppid\(\)

> 该方法只能在unix/Linux/Mac上运行，windows不可以运行。
>
> 程序执行到fork\(\)时，操作系统会创建一个新进程（子进程），并把父进程的所有信息赋值到子进程中。
>
> 这个方法很特殊，会有两次返回，分别在子进程和父进程返回一次，子进程永远返回0，父进程返回进程的id.
>
> getpid\(\):返回当前进程的id
>
> getppid\(\):返回当前进程父进程的id。

实例：

```
import os
id=os.fork()
index=0
if id<0:
    print("子进程创建失败了")
elif id==0:
    index+=1
    print("我是子进程（%d），我的父进程是：%d,index=%d"%(os.getpid(),os.getppid(),index))
else:
    index += 1
    print("我是父进程：%d，我的子进程是：%d,index=%d"%(os.getpid(),id,index))
```

结果：

```
我是父进程：9735，我的子进程是：9736,index=1
我是子进程（9736），我的父进程是：9735,index=1
```

从上面实例也可以看出：每个进程的所有数据（包括全局变量）都各持一份，互不影响。

多次fork\(\)可发现：父子进程执行顺序没有规律，完全取决于操作系统的调度算法。

