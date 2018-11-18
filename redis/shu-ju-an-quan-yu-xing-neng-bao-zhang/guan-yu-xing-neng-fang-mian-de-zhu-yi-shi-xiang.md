习惯了关系数据库的用户在刚开始使用Redis的时候，通常会因为Redis带来的上百倍的性能提升而感到欣喜若狂，却没有认识到Redis性能实际上还可以进一步的提高。虽然上一节介绍的非事务型流水线可以尽可能地减少应用程序和Redis之间的通信往返次数，但是对于一个已经存在的应用程序，我们应该如何判断这个程序能否被优化呢？我们又应该如何对它进行优化呢？

要对Redis的性能进行优化，用户首先需要弄清楚各种类型的Redis命令到底能跑多快，而这一点可以通过调用Redis附带的性能测试程序redis-benchmark来得知，下面清单展示了一个相应的例子。如果有兴趣的话，读者也可以试着用redis-benchmark来了解Redis在自己服务器上的各种性能特征：

在装有英特尔酷睿2双核2.4GHz处理器的台式电脑上运行redsi-benchmark

```
#给定‘-q’选项可以让程序简化输出结果，给定‘-c 1’选项让程序只使用一个客户端来进行测试
$ redis-benchmark -c 1 -q
PING (inline):34246.57 requests per second
Pind:34843.32 requests per second
MSET (10 keys):24213 08 request per second
SET: 32467.53 request per second
GET: 33112.59 request per second
INCR: 32679.74 request per second
LPUSH: 33333.33 request per second
LPOP: 33670.04 request per second
SADD: 33222.59 request per second
SPOP: 34482.76 request per second
LPUSH (again, in order to bench LRNAGE):33222.59  request per second
LRANGE (first 100 elements):22988.51 request per second
LRANGE (first 300 elements):13888.89 request per second
LRANGE (first 450 elements):11061.95 request per second
LRANGE (first 600 elements):9041.59 request per second
```

redis-benchmark的运行结果展示了一些常用Redis命令在1秒内可以执行的此时。如果用户在不给定任何参数的情况下运行redis-benchmark，那么redis-benchmark将使用50个客户端来进行性能测试，但是为了在redis-benchmark和我们自己的客户端之间进行性能对比，让redis-benchmark只使用一个客户端要比使用多个客户端更方一些。

在考察redis-benchmark的输出结果时，切记不要将输出结果看做是用于程序的实际性能，这是因为redis-benchmark不会处理执行命令所获得的命令回复，所以它节约了大量用于对命令回复进行语法分析的时间。在一般情况下，对于只使用单个客户端的redis-benchmark来说，根据被调用命令的复杂度，一个不使用流水线的Python客户端的性能大概只有redis-benchmark所示性能的50%~60%。

另一方面，如果你发现自己客户端的性能只有redis-benchmark所示性能的25%~30%，或者客户端向你返回了”Cannot assign requested address“\(无法分配指定的地址\)错误，那么你可能是不小心在每次发送命令时都创建了新的连接。

下表列出了只使用单个客户端的redis-benchmark与Python客户端之间的性能对比结果，并介绍了一些常见的造成客户端性能底下或者出错的原因：

> 比较了Redis在通常情况下的性能表现以及redis-benchmark使用单客户端进行测试时的结果，并说明了一些可能引起性能问题的原因

| 性能或者错误 | 可能的原因 | 解决方法 |
| :--- | :--- | :--- |
| 单个客户端的性能达到redis-benchmark的50%~60% | 这是不使用流水线的预期性能无 | 无 |
| 单个客户端的性能达到redis-benchmark的25%~30% | 对于每个命令或者每组命令都创建了新的连接 | 重用已有的Redis连接 |
| 客户端返回错误：”Cannot assign requested address“\(无法分配指定的地址\) | 对于每个命令或者每组命令都创建了新的连接 | 重用已有的Redis连接 |

尽管上表列出的性能问题以及问题的解决方法都非常简短，但绝大部分常见的性能问题都是由表格中列出的原因引起的（另一个引起性能问题的原因是以不正确的方式使用Redis的数据结构）。如果读者遇到了难以解决的性能问题，或者遇到上表没有介绍的性能问题，那么读者可以考虑通过1.4节介绍的方法来寻求帮助。

大部分Redis客户端库都提供了某种级别的内置连接池。以Python的Redis客户端为例，对于每个Redis服务器，用户只需要创建一个redis.Redis\(\)对象，该对象就会按需创建连接、重用已有的连接并关闭超时的连接（在使用多个数据库的情况下，即使客户端只连接了一个Redis服务器，它也需要为每一个被使用的数据库创建一个连接），并且Python客户端的连接池还可以安全地应用于多线程环境和多进程环境。

