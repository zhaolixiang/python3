# 1、多线程使用的必要性

在引入多线程之前，我们先来看一个非常简单的实例。

实例：

```
#单线程实例
import time

def mark(index):
    print("Mark的帅，远近闻名,第%d次传播"%index)
    #暂停一秒，不然看不到效果哦
    time.sleep(1)

if __name__=="__main__":
    for i in range(6):
        mark(i)
```

结果：按照顺序依次打印

![](/assets/1、单线程.gif)

上面是单线程显示效果，现在我们来用多线程处理一下。

实例：

```

```

效果：

