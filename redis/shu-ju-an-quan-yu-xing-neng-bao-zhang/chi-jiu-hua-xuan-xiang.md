Redis提供了两种不同的持久化方法来将数据存储到硬盘里面。一种方法叫快照（snapshotting），它可以将存在于某一时刻的所有数据都写入硬盘里面。另一种方法叫只追加文件（append-only file,AOF），它会在执行写命令时，将被执行的写命令复制到硬盘里面。这两种持久化方法即可以同时使用，又可以单独使用，在某些情况下甚至可以两种方法都不使用，具体选择哪种持久化方法需要根据用户的数据以及应用的决定。

将内存中的数据存储到硬盘的一个主要原因是为了在之后重用数据，或者是为了防止系统故障而将数据备份到一个远程位置。另外，存储在Redis里面的数据有可能是经过长时间计算得出的，或者有程序在使用Redis存储的数据进行计算，所有用户希望可以将这些数据存储起来以便之后使用，这样就不必再重新计算了。对于一些Redis应用来说，“计算”可能只是简单地将另一个数据库的数据复制到Redis里面，但对于另外一些Redis应用来说，Redis存储的数据可能是根据数十亿行日志进行聚合分析得出的结果。

两组不同的配置选项控制着Redis将数据写入硬盘里面的方法。下面代码展示了这些配置选项以及他们的实例配置值：

```
#快照持久化选项
save 60 1000
stop-writes-on-bgsave-error no
rdbcompression yes
dbfilename dump.rdb

#AOF持久化选项
appendonly no
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

#共享选项，这个选项决定了快照文件和AOF文件的保存位置。
dir ./
```



