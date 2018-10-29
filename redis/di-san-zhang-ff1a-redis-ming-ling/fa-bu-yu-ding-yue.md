一般来说，发布于订阅（又称pub/sub）的特点是订阅者（listener）负责订阅频道（channel），发送者（publisher）负责向频道发送二进制字符串消息（binary string message）。每当有消息被发送至给定频道时，频道的所有订阅者都会收到消息。我们也可以把频道看作是电台，其中订阅者可以同时收听多个电台，而发送者则可以在任何电台发送信息。

本节将对发布与订阅的相关操作进行介绍，阅读这一节可以让读者学会怎样使用发布与订阅的相关命令，并了解到为什么本书在之后的章节里面会使用其他相似的解决方案来代替Redis提供的发布与订阅。

下表展示了Redis提供的5个发布与订阅命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| subscribe | subscribe channel \[channel ...\] | 订阅给定的一个或多个频道 |
| unsubscribe | unsubscribe \[channnel channel ...\] | 退订给定的一个或多个频道，如果执行时没有给定任何频道，那么退订所有频道 |
| publish | publish channel message | 向给定频道发送消息 |
| psubscribe | psubscribe pattern \[pattern ...\] | 订阅与给定模式相匹配的所有频道 |
| punsunscribe | punsunscribe \[pattern \[pattern ...\]\] | 退订给定的模式，如果执行时没有给定任何模式，那么退订所有模式。 |

考虑到publish命令和subscribe命令在Python客户端的实现方式，一个比较简单的演示发布与订阅的方法，就像下面代码清单那样使用辅助线程（helper thread）来执行publish命令：

```
import redis  # 导入redis包包
import time,threading


# 与本地redis进行链接，地址为：localhost，端口号为6379

r = redis.StrictRedis(host='localhost', port=6379)

def publisher(n):
    #函数在开始执行时会先休眠，让订阅者有足够的事件来连接服务器并监听消息
    time.sleep(1)
    for i in range(n):
        r.publish('channel',i)
        #在发送消息之后进行短暂的休眠，让消息可以一条接一条地出现
        time.sleep(1)

def run_pubsub():
    #启动发送者线程，并让它发送三条消息
    threading.Thread(target=publisher,args=(3,)).start()
    #创建发布于订阅对象，并对它订阅给定的频道
    pubsub=r.pubsub()
    pubsub.subscribe(['channel'])
    count=0
    #通过遍历函数pubsub.listen()的执行结果来监听订阅消息
    for item in pubsub.listen():
        #打印接收到的每条消息
        print(item)
        count+=1
        if count==4:
            pubsub.unsubscribe()
        if count==5:
            break

if __name__ == '__main__':
    run_pubsub()

```

运行结果：

```
{'type': 'subscribe', 'pattern': None, 'channel': b'channel', 'data': 1}
{'type': 'message', 'pattern': None, 'channel': b'channel', 'data': b'0'}
{'type': 'message', 'pattern': None, 'channel': b'channel', 'data': b'1'}
{'type': 'message', 'pattern': None, 'channel': b'channel', 'data': b'2'}
{'type': 'unsubscribe', 'pattern': None, 'channel': b'channel', 'data': 0}
```

> 在刚开始订阅一个频道的时候，客户端会接收到一条关于被订阅频道的反馈。
>
> 在退订频道时，客户端会接受到一条反馈信息，告知被退订的是哪个频道，以及客户端目前仍在订阅的频道数量。



