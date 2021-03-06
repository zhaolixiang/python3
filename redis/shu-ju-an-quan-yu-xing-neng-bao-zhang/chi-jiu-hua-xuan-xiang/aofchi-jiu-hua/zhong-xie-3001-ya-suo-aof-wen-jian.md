# 重写、压缩AOF文件

AOF持久化既可以将丢失数据的时间区间降低至1秒（甚至不丢失任何数据），又可以在极端的时间内完成定期的持久化操作，那么我们有什么理由不使用AOF持久化呢？但是这个问题实际上并没有那么简单，因为Redis会不断地将被执行的写命令记录到AOF文件里面，所有随着Redis不断运行，AOF文件的体积会不断增长，在极端情况下，体积不断增大的AOF文件甚至可能会用完磁盘的所有可用空间。还有另一个问题是，因为Redis在重启之后需要通过重新执行AOF文件记录的所有写命令的还原数据集，所以如果AOF文件的体积非常大，那么还原操作执行的时间就可能会非诚长。

为了解决AOF文件体积不断增大的问题，用户可以向Redis发送bgrewriteaof命令，这个命令会通过移除AOF文件中的冗余命令来重写（rewrite）AOF文件，使AOF文件的体积变得尽可能地小。bgrewriteaof的工作原理和bgsave创建快照的工作原因非常相似：Redis会创建一个子进程，然后由子进程负责对AOF文件进行重写。因为AOF文件重写也需要用到子进程，所以快照持久化因为创建子进程而导致的性能问题和内存占用问题，在AOF持久化中也同样存在。更糟糕的是，如果不加以控制的话，AOF文件的体积可能会比快照恩简的体积大好几倍，在进行AOF重写并删除旧AOF文件的时候，删除一个体积达到数十GB大的旧AOF文件可能会导致操作系统挂起数秒。

跟快照持久化可以通过设置save选项来自动执行bgsave一样，AOF持久化也可以通过设置auto-aof-rewrite-percentage选项和auto-aof-rewrite-min-size选项来自动执行bgrewriteaof。举个例子，假设用户对Redis设置了配置选项auto-aof-rewrite-percentage 100和auto-aof-rewrite-min-size 64mb，并且启用了AOF持久化，那么当AOF文件的体积大于64MB，并且AOF文件的体积比上一次重写之后的体积大了至少一倍（100%）的时候，Redis将执行bgrewriteaof命令。如果aof重写执行得过于频繁地话，那么用户可以考虑将auto-aof-rewrite-percentage选项的值设置为100以上，这种做法可以让Redis在AOF文件的体积变得更大之后才执行重写操作，不过也会让Redis在启动时还原数据集所需的时间变得更长。

无论是使用AOF持久化还是快照持久化，将数据持久化到硬盘上都是非常有必要的，但除了进行持续化之外，用户还必须对持久化所得的文件进行备份（最好是备份到不同的地方），这样才能尽量避免数据丢失事故发生。如果条件允许的话，最好能够将快照文件和最新重写的AOF文件备份到不同的服务器上面。

通过使用AOF持久化或者快照持久化，用户可以在系统重启或者崩溃的情况下仍让保留数据。随着负载量的上升、或者数据的完整性变得越来越重要时，用户可能需要使用复制性。

