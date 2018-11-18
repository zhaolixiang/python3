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



