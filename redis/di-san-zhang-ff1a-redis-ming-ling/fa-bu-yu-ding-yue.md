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



