之前章节首次介绍multi和exec的时候讨论过它们的”事务“性质：被multi和exec包裹的命令在执行时不会被其他客户端打扰。而使用事务的其中一个好处就是底层的客户端会通过使用流水线来提高事务执行的性能。本节将介绍如何在不使用事务的情况下，通过使用流水线来进一步提升命令的执行性能。

之前章节曾经介绍过一些可以接受多个参数的添加命令和更新命令，如mget、mset、hmget、hmset、rpush、lpush、sadd、zadd等。这些命令简化了那些需要重复执行相同命令的操作，并且极大地提升了性能。尽管效果可能没有以上提到的命令那么显著，但使用非事务型流水线同样可以获得相似的性能提升，并且可以让用户同时执行多个不同的命令。

在需要执行大量命令的情况下，即使命令实际上并不需要放在事务里面执行，但是为了通过一次发送所有命令来减少通信次数并降低延迟值，用户也可能会将命令包裹在multi和exec里面执行。遗憾的是，multi和exec并不是免费的：它们也会消耗资源，并且可能会导致其他重要的命令被延迟执行。不过好消息是，我们实际上可以在不使用multi和exec的情况下，获得流水线代理的所有好处。之前章节使用了一下语句来在Python中执行multi和exec命令：

```
pipe=conn.pipeline()
```

如果用户在执行pipeline\(\)时传入True作为参数，或者不传入任何参数，那么客户端将使用multi和exec包裹起用户要执行的所有命令。另一方面，如果用户在执行pipeline\(\)时传入False为参数，那么客户端同样会像执行事务那样收集起用户要执行的所有命令，只是不再使用multi和exec包裹这些命令。如果用户需要向Redis发送多个命令，并且对于这些命令来说，一个命令的执行结果并不会影响另一个命令的输入，而且这些命令也不需要以实物的方式来执行的话，那么我们可以通过向pipeline\(\)方法传入False来进一步提升Redis的整体性能。让我们来看一个这方面的例子。

前面章节曾经编写并更新过一个名为update\_token\(\)函数，它负责记录用户最近浏览过的商品以及用户最近访问过的页面，并更新用户的登录cookie。下面代码是之前展示过得更新版update\_token\(\)函数，这个函数每次执行都会调用2个或者5个Redis命令，使得客户端和Redis之间产生2次或5次通信往返。

```
import time


def update_token(conn,token,user,item=None):
    #获取时间戳
    timestamp=time.time()
    #创建令牌和已登陆用户之间的映射
    conn.hset('login:',token,user)
    #记录令牌最后一次出现的时间
    conn.zadd('recent:',token,timestamp)
    if item:
        #把用户浏览过的商品记录起来
        conn.zadd('viewed:'+token,item,timestamp)
        #移除旧商品，只记录最新浏览的25件商品
        conn.zremrangebyrank('viewed:'+token,0,-26)
        #更新给定商品的被浏览慈善
        conn.zincrby('viewed:',item,-1)
```

如果Redis和Web服务器通过局域网进行连接，那么他们之前的每次通信往返大概需要耗费一两毫秒，因此需要进行2次或者5次通信往返的update\__token\(_\)函数大概需要花费2~10毫秒来执行，按照这个速度计算，单个Web服务器线程每秒可以处理100~500个请求，尽管这种速度已经非常可观了，但是我们还可以在这个速度的基础上更新一步：通过修改update\_token\(\)函数，让它创建一个非事务型流水线，然后使用这个流水线来发送所有请求，这样我们就的带了下面代码：

```
import time


def update_token_pipeline(conn,token,user,item=None):
    #获取时间戳
    #设置流水线
    pipe=conn.pipeline(False)
    timestamp=time.time()
    #创建令牌和已登陆用户之间的映射
    conn.hset('login:',token,user)
    #记录令牌最后一次出现的时间
    conn.zadd('recent:',token,timestamp)
    if item:
        #把用户浏览过的商品记录起来
        conn.zadd('viewed:'+token,item,timestamp)
        #移除旧商品，只记录最新浏览的25件商品
        conn.zremrangebyrank('viewed:'+token,0,-26)
        #更新给定商品的被浏览慈善
        conn.zincrby('viewed:',item,-1)
    pipe.execute()
```

通过将标准的Redis连接替换成流水线连接，程序可以将通信往返的次数减少至原来的1/2到1/5，并将update\_token\_pipeline\(\)函数的预期执行时间降低1~2毫秒。按照这个速度来计算的话，如果一个Web服务器只需要执行update\_token\_pipeline\(\)来更新商品的浏览信息，那么这个Web服务器每秒可以处理500~1000个请求。从理论上来看，update\_token\_pipeline\(\) 函数的效果非常棒，但是它的实际运行速度又是怎样的呢？

为了回答这个问题，我们将对update\_token\(\)函数和update\_token\_pipeline\(\)函数进行一些简单的测试。我们将分别通过快速低延迟网络和慢速高延迟网络来访问同一台机器，并测试运行在机器上面的Redis每秒可以处理的请求数量。下面代码展示了性能测试函数，这个函数会在给定的时限内重复执行update\_token\(\)函数或者update\_token\_pipeline\(\)函数，然后计算被测试的函数每秒执行了多少次。

```

```

