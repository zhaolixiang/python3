# 最新日志

在构建一个系统的时候，判断哪些信息需要被记录是一件困难的事情：需要记录用户的登录和退出行为吗？需要记录用户修改账号信息的时间吗？还是只记录错误和异常就可以了？虽然我没有办法替你回答这些问题，但我可以向你提供一种将最新出现的日志消息以列表的形式存储到Redis里面的方法，这个列表可以帮助及你随时了解最新出现的日志都是什么样子的。

下面代码的log\_recent\(\)函数展示了将最新日志记录到Redis里面的方法：为了维持一个包含最新日志的列表，程序使用lpush命令将日志消息推入一个列表里面。之后，如果我们想要查看已有日志消息的话，那么可以使用lrange命令来取出列表中的消息。除了lpush之外，函数还加入了一些额外的代码，用于命名不同的日志消息队列，并根据文意的严重性对日志进行分级，如果你觉得自己并不需要这些附加功能的话，也可以将相关代码删除掉，只保留基本的日志添加功能。

```
#设置一个字典，将大部分日志的安全级别映射为字符串
import logging
import time

SEVERITY={
    logging.DEBUG:'debug',
    logging.INFO:'info',
    logging.WARNING:'warning',
    logging.ERROR:'debug',
    logging.CRITICAL:'critical',
}
SEVERITY.update((name,name) for name in SEVERITY.values())

def log_recent(conn,name,message,severity=logging.INFO,pipe=None):
    #尝试将日志的安全级别准还为简单的字符串
    severity=str(SEVERITY.get(severity,severity)).lower()
    #创建负责存储消息的键
    destination='recent:%s:%s'%(name,severity)
    #将当前时间添加到消息里面，用于记录消息的发送时间
    message=time.asctime()+'  '+message
    #使用流水线来将通信往返次数降低为一次
    pipe=pipe or conn.pipeline()
    #将消息添加到日志列表的最前面
    pipe.lpush(destination,message)
    #对日志列表进行修建，让它只包含最新的100条消息
    pipe.ltrim(destination,0,99)
    #执行两个命令
    pipe.execute()
```

除了那些将日志的安全级别转换为字符串（如info和debug）的代码之外，log\_recent\(\)函数的定义非常简单：基本上就是一个lpush加上一个ltrim。现在你已经知道怎样记录最新出现的日志了，是时候来了解一下该如何记录最常出现（也是最重要的）日志消息了。

