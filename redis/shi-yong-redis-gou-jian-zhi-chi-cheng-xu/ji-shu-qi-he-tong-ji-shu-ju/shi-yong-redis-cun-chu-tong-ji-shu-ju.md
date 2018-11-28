首先需要说明的一点是，为了统计数据存储到Redis里面，笔者曾经实现过5种不同的方法，本节介绍的方法综合了这5种方法里面的众多优点，具有非常大的灵活性和可扩展性。

本节所展示的存储统计数据的方法，在工作方式上与上节介绍的log\_common\(\)函数类似：这两者存储的数据记录的都是当前这一小时以及前一小时所产生的事情。另外，本节介绍的方法会记录最小值、最大值、平均值、标准差、样本数量以及所有被记录值之和等众多信息，以便不时之需。

对于一种给定的上下文和类型，程序将使用一个有序集合来记录这个上下文以及这个类型的最小值、最大值、样本数量、值的和、值的平方之和等信息，并通过这些信息来计算平均值以及标准差。程序将值存储在有序集合里面并非是为了按照分值对成员进行排序、而是为了对存储着统计信息的有序集合和其他有序集合进行并集计算，并通过min和max这两个聚合函数来筛选相交的元素。下表展示了一个存储统计数据的有序集合实例，它记录了ProfilePage\(个人简历\)上下文的AccessTime（访问时间）统计数据。

| 表名：starts:ProfilePage:AccessTime | 类型：zset |
| :--- | :--- |
| min | 0.035 |
| max | 4.958 |
| sunsq | 194.268 |
| sum | 258.973 |
| count | 2323 |

既然我们已经知道了程序要存储的是什么类型的数据，那么接下来要考虑的就是如何将这些数据写到数据结构里面了。

下面代码展示了负责更新统计数据的代码。和之前介绍过的常见日志程序一样，统计程序在写入数据之前会进行检查，确保被记录的是当前这小时的统计数据，并将不属于当前这一小时的旧数据进行归档。在此之后，程序会构建两个临时有序集合，其中一个用于保存最小值，而另一个则用于保存最大值然后使用zunionstore命令以及它的两个聚合函数min和max，分别计算两个临时有序集合与记录当前统计数据的有序集合之前的并集结果。通过使用zunionstore命令，程序可以快速的更新统计数据，而无须使用watch去监视可能会频繁进行更新的存储统计数据的键，因为这个键可能会频繁地进行更新。程序在并集计算完毕之后就会删除那些临时有序集合，并使用zincrby命令对统计数据有序集合里面的count、sum、sumsq这3个成员进更新。

```
import datetime
import time
import uuid

import redis


def update_status(conn,context,type,value,timeout=5):
    #负责存储统计数据的键
    destination='stats:%s:%s'%(context,type)
    #像common_log()函数一样，处理当前这一个小时的数据和上一个小时的数据
    start_key=destination+':start'
    pipe=conn.pipeline(True)
    end=time.time()+timeout
    while time.time()<=end:
        try:
            pipe.watch(start_key)
            now=datetime.utcnow().timetuple()
            # 像common_log()函数一样，处理当前这一个小时的数据和上一个小时的数据
            hour_start=datetime(*now[:4]).isoformat()

            existing=pipe.get(start_key)
            pipe.multi()
            if existing and existing<hour_start:
                # 像common_log()函数一样，处理当前这一个小时的数据和上一个小时的数据
                pipe.rename(destination,destination+':last')
                pipe.rename(start_key,destination+':pstart')
                pipe.set(start_key,hour_start)

            tkey1=str(uuid.uuid4())
            tkey2=str(uuid.uuid4())
            #将值添加到临时键里面
            pipe.zadd(tkey1,'min','value')
            pipe.zadd(tkey2,'max','value')
            #使用聚合函数min和max，对存储统计数据的键以及两个临时键进行并集计算
            pipe.zunionstore(destination,[destination,tkey1],aggregate='min')
            pipe.zunionstore(destination,[destination,tkey2],aggregate='max')

            #删除临时键
            pipe.delete(tkey1,tkey2)
            #对有序集合中的样本数量、值的和、值的平方之和3个成员进行更新。
            pipe.zincrby(destination,'count')
            pipe.zincrby(destination,'sum',value)
            pipe.zincrby(destination,'sumsq',value*value)

            #返回基本的计数信息，以便函数调用者在有需要时做进一步的处理
            return pipe.execute()[-3:]

        except redis.exceptions.WatchError:
            #如果新的一个小时已经开始，并且旧的数据已经被归档，那么进行重试
            continue
```

update\__status\(\)函数的前半部分代码基本上可以忽略不看，因为它们和上节介绍的log\_common\(\)函数用来轮换数据的代码几乎一模一样，而update\_\_status\(\)函数的后半部分则做了我们前面描述过的事情：程序首先创建两个临时有序集合，然后使用适当的聚合函数，对存储统计数据的有序集合以及两个临时有序集合分别执行zunionstore命令；最后，删除临时有序集合，并将并集计算所得的统计数据更新到存储统计数据的有序集合里面。update_\_status\(\)函数展示了将统计数据存储到有序集合里面的方法，但如果想要获取统计数据的话，又应该怎么做呢？

下面代码展示了程序取出统计数据的方法：程序会从记录统计数据的有序集合里面取出所有被存储的值，并计算出平均值和标准差。其中，平均值可以通过值的和\(sum\)除以取样数量\(count\)来计算得出；而标准差的计算则更复杂一些，程序需要多做一些工作才能根据已有的统计信息计算出标注差，但是为了简洁起见，这里不会解释计算标准差时用到的数学知识。

```

```











































