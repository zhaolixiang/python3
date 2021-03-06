# 快照持久化

Redis可以通过创建快照来获得存储在内存里面的数据在某个时间点上的副本。在创建快照之后，用户可以对快照进行备份，可以将快照复制到其它服务器从而创建具有相同数据的服务器副本，还可以将快照留在原地以便重启服务器时使用。

根据配置，快照将被写入dbfilename选项指定的文件里面，并存储在dir选项指定的路径上面。如果在新的快照文件创建文笔之前，Redis、系统或者硬件上三者之中的任意一个崩溃了，那么Redis将丢失最近一次创建快照之后写入的所有数据。

举个例子，假设Redis目前在内存里面存储了10GB的数据，上一个快照是在下午2：35开始创建的，并且已经创建成果。下午3：06时，Redis又开始创建新的快照，并且在下午3：08快照文件创建完毕之前，有35个键进行了更新。如果在下午3：06至3：08期间，系统发生崩溃，导致Redis无法完成新快照的创建工作，那么Redis将丢失下午2：35之后写入的所有数据。

创建快照的办法有以下几种：

* 客户端可以通过向Redis发送bgsave命令创建一个快照。对于支持bgsave命令的平台来说（基本上所有的平台都支持，除了Windows平台），Redis会调用fork来创建一个子进程，然后子进程负责将快照写入硬盘，而父进程则继续处理命令请求。

> fork：当一个进程创建子进程的时候，底层的操作系统会创建该进程的一个副本。在Unix和类Unix系统上面，创建子进程的操作会进行如下优化：在刚开始的时候，父进程共享相同的内存，直到父进程或者子进程对内存进行了写入之后，对呗写入内存的共享才会结束。

* 客户端还可以通过向Redis发singsave命令来创建一个快照，接到save命令的Redis服务器在快照创建完毕之前将不再响应任何其他命令。save命令并不常用，我们通常只会在没有足够内存去执行bgsave命令的情况下，又或者即使等待持久化操作执行完毕也无所谓的情况下，才会使用这个命令。
* 如果用户设置了save配置选项，比如save 60 10000，那么从Redis最近一次创建快照之后开始算起，当【60秒之内有10 000次写入】这个条件满足时，Redis就会自动触发bgsave命令。如果用户设置了多个save配置选项，那么当任意一个save的配置选项所设置的条件被满足时，Redis就会触发一次bgsave命令。

* 当Redis通过shutdown命令接收到关闭服务器的请求时，或者接受到标准term信号时，会执行一个save命令，阻塞所有客户端，不再执行客户端发送的任何命令，并在save命令执行完毕之后关闭服务器。

* 当一个Redis服务器连接另一个Redis服务器，并向对方发送sync命令来开始一次复制操作的时候，如果主服务器目前没有在执行bgsave操作，或者主服务器并非刚刚执行完bgsave操作，那么主服务器就会执行bgsave命令。

在只使用快照持久化来保存数据时，一定要记住：如果系统真的发生崩溃，用户将丢失最近一次生成快照之后更改的所有数据。因此，快照持久化只适用于那些丢失一部分数据也不会造成问题的应用程序，而不能接受这种数据损失的应用程序则可以考虑使用AOF持久化。接下来将展示几个使用快照持久化的场景，读者可以从中学习到如何通过修改配置来获得资金想要的快照持久化的行为。





