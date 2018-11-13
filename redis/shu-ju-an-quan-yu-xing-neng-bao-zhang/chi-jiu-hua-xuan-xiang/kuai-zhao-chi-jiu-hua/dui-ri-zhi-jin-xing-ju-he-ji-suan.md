在对日志文件进行聚合计算或者对页面浏览量进行分析的时候，我们需要唯一考虑的就是：如果Redis因为崩溃而未能成功创建新的快照，那么我们能够承受丢失多长时间以内产生的新数据。如果丢失一个小时之内产生的数据是可以接受的，那么可以使用配置值save 3600 1（3600秒为1小时）。在决定好了持久化配置值之后，另一个需要解决的问题就是如何恢复因为故障而被终端的日志处理操作。

在进行数据恢复时，首先要做的就是弄清楚我们丢失了哪些数据。为了弄明白这一点，我们需要在处理日志的同时记录被处理日志的相关信息。

下面代码展示了一个用于处理新日志的函数，该函数有3个参数，他们分别是：一个Redis链接；一个存储日志文件的路径；待处理日志文件中各个行（line）的回调函数。这个函数可以在处理日志文件的同时，记录被处理的日志文件的名字以及偏移量。

```
import os

import redis  # 导入redis包包

#日志处理函数接收的其中以恶搞参数为回调函数
#这个回调函数接收一个Redis连接和一个日志行作为参数，
#并通过调用流水线呢对象的方法来执行Redis命令
def process_log(conn,path,callback):
    #获取文件当前的处理进度
    current_file,offset=conn.mget('progress.file','progress.position')
    pipe=conn.pipeline()

    #通过使用闭包（closur）来减少重复代码
    def update_progress():
        pipe.mset({'progress.file':fname,'progress:position':offset})
        #这个语句负责执行实际的日志更细操作，并将日志文件的名字和目前的处理器进度记录到Redis里面
        pipe.execute()

    #有序的遍历各个日志文件
    for fname in sorted(os.listdir(path)):
        #略过所有已处理的日志文件
        if fname <current_file:
            continue

        inp=open(os.path.join(path,fname),'rb')
        #在接着处理一个因为系统崩溃而未能完成处理的日志文件时，略过已处理的内容
        if fname==current_file:
            inp.seek(int(offset,10))
        else:
            offset=0

        current_file=None

        #枚举函数遍历一个由文件行足哼的序列，并返回任意多个二元组

        #每个二元祖包含了行号lno和行数据line，其中行号从0开始
        for lno,line in enumerate(inp):
            #处理日志
            callback(pipe,line)
            #更细已处理内容的偏移量
            offset+=int(offset)+len(line)

            #每当处理完1000个日志行或者处理完 整个日志文件的时候，都更新一次文件的处理进度
            if not (lno+1) %1000:
                update_progress()
        update_progress()

        inp.close()


```

通过将日志的处理进度记录到Redis里面，城西可以在系统崩溃之后，根据进度记录继续执行之前未完成的处理工作。而通过事务流水线，程序保证日志的处理结果和处理进度总是会同时被记录到快照文件里面。

