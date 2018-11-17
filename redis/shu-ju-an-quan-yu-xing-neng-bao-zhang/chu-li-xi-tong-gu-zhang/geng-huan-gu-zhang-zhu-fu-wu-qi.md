# 更换故障主服务器

在运行一组同时使用复制和持久化的Redis服务器时，用户迟早都会遇上某个或某些Redis服务器停止运行的情况。造成故障的原因可能是硬盘驱动器出错、内存出错或者电量耗尽，但无论服务器因为何种原因出现故障，用户最终都要对发生故障的服务器进行更换。现在让我们来看看，在拥有一个主服务器和一个从服务器的情况下，更换主服务器的具体步骤。

假设A、B两台机器都运行着Redis，其中机器A的Redis为主服务器，而机器B的Redis为从服务器。不巧的是，机器A刚刚因为某个暂时无法修复的故障而断开网络连接，因此用户决定将同样安装了Redis的机器C用作新的主服务器。

更换服务器的计划非常简单，首先向服务器B发送一个save命令，让它创建一个新的快照文件，接着将这个快照文件发送给机器C，并在机器C上面启动Redis，最后，让机器B成为机器C的从服务器。下面代码展示了更换服务器时用到的各个命令：

```
通过VPN网络连接机器B
user@vpm-master ~:$ssh root@machine-b.vpn
Last Login:wed Mar 28 15:21:06 2012 from ...

#启动命令行Redis客户端来执行几个简单的操作
root@machine-b ~:$redis-cli
#执行save命令，并在命令完成之后，使用quit命令退出客户端
redis 127.0.0.1:6379> save
OK
redis 127.0.0.1:6379> quit

#将快照文件发送至新的主服务器：机器C
root@machine-b ~:$scp \ 
> /var/local/redis/dump.rdb machine-c.vpn:/var/local/redis/dump.rdb

#连接新的主服务器并启动Redis
root@machine-b ~:$ssh machine-c.vpn
Last login:True Mar 27 12:42:31 2012 from ...
root@machine-c ~:$sudo /etc/init.d/redis-server start
Starting Redis server...
root@machine-c ~:$ exit
root@machine-b ~:$redis-cli

#告知机器B的Redis，让它将机器C用作新的主服务器
redis 127.0.0.1:6379>slaveof machine-c.vpn 6379
OK
redis 127.0.0.1:6379>quit
root@machine-b ~:$exit
user@vpm-master ~:$
```

另一种创建新的主服务器的方法，就是将从服务器升级为主服务，并为升级后的主服务器创建从服务器。

以上列举的两种方法都可以让Redis回到之前的一个主服务器和一个从服务器的状态，而用户接下来要做的就是更新客户端的配置，让他们去读写正确的服务器。除此之外，如果用户需要重启Redis的话，那么可能还需要对服务器的持久化配置进行更新。

> Redis Sentinel
>
> Redis Sentinel可以监视指定的Redis主服务器及其属下的从服务器，并在主服务器下线时自动进行故障转移。后面章节会介绍。

在接下来的一节中，我们将介绍一种保证数据安全所必不可少地功能，该功能可以在多个客户端同时对相同的数据进行写入时，防止数据出错。



