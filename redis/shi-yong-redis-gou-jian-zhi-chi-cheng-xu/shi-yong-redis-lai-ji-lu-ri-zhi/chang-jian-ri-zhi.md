# 常见日志

如果实际运行一下log\_recent\(\)函数的话，你就会发现，尽管log\_recent\(\)函数非常适用于记录当前发生的事情，但它并不擅长告诉你哪些消息时重要的，哪些消息是不重要的。为了解决这个问题，我们可以让程序记录特定消息出现的频率，并根据出现频率的高低来决定消息的排列顺序，从而帮助我们找出最重要的消息。

下面代码的log\_comon\(\)函数展示了记录并轮询最常见日志消息的方法：程序会将消息作为成员存储的有序集合里面，并将消息出现的频率设置为成员的分值。为了确保我们看见的常见消息都是最新的，程序会以每小时一次的频率对消息进行轮换，并在轮换日志的时候保留上一个小时记录的常见消息，从而防止没有任何消息存在的情况出现。

```

def log_common(conn,name,message,severity=logging.INFO,timeout=5):
    # 尝试将日志的安全级别准还为简单的字符串
    severity = str(SEVERITY.get(severity, severity)).lower()
    #负责存储近期的常见日志消息的键
    destination = 'common:%s:%s' % (name, severity)
    #因为程序每小时需要轮换一次日志，所以它使用一个键来记录当前所处的小时数
    start_key=destination+':start'
    # 使用流水线来将通信往返次数降低为一次
    pipe = conn.pipeline()
    end=time.time()+timeout
    while time.time()<end:
        try:
            #当记录当前小时数的键进行监视，确保轮换操作可以正确的执行
            pipe.watch(start_key)
            #取得当前时间
            now=datetime.utcnow().timetuple()
            #取得当前所处的小时数
            hour_start=datetime(*now[:4].isoformat())

            existing=pipe.get(start_key)
            #创建一个事务
            pipe.multi()
            #如果这个常见日志消息列表记录的是上个小时的日志。。
            if existing and existing<hour_start:
                #将这些旧的常见日志消息归档
                pipe.rename(destination,destination+':last')
                pipe.rename(start_key,destination+':pstart')
                #更新当前所处的小时数
                pipe.set(start_key,hour_start)
            elif not existing:
                pipe.set(start_key,hour_start)
            #对记录日志出现次数的计数器执行自增操作
            pipe.zincrby(destination,message)
            #log_recent()函数负责记录日志并调用execute()函数
            log_recent(pipe,name,message,severity,pipe)
            return
        except redis.exceptions.WatchError:
            continue
```



