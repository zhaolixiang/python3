WebSocket protocol是HTML5定义的一种新的标准协议（RFC6455），它实现了浏览器与服务器的双全工通信（full-duplex）。

# 1、WebSocket的应用场景

传统的HTTP和HTML技术使用客户端主动向服务器发送请求并获取回复。但是随着即时通讯需求的增多，这样的通信模式有时并不能满足应用的需求。

WebSocket与普通的Socket通讯类似，它打破了原来HTTP的Request和Response一对一的通信模型，同时打破了服务器只能被动地接受客户端请求的应用场景。也许读者听说过Ajax、Long poll等基于传统HTTP的动态客户端技术，但这些技术无不采用轮询技术，耗费了大量的网络带宽和计算资源。

而WebSocket正是为了应对这样的场景而制定的HTML5标准，相对于普通的Socket通信，WebSocket又在应用层定义了基本的交互流程，使得Tornado这样的服务器框架和JavaScript客户端可以构建出标准的WebSocket模块。

总结WebSocket的特点如下：

* WebSocket适合服务端主动推送的场景。
* 相对于Ajax和Long poll等技术，WebSocket通信模型更高效。
* WebSocket仍然与HTTP完成Internet通信。
* 因为是HTML5的标准协议，所以不受企业防火墙的拦截。

# 2、WebSocket的通信原理

WebSocket的通信原理是在客户端与服务器之间建立TCP持久链接，从而使得当服务器有消息需要推送给客户端时能够进行即时通信。

虽然WebSocket不是HTTP，但由于在Internet上HTML本事是由HTTP封装并进行传输的，所以WebSocket仍然需要与HTTP进行协作。IETF在RFC6455中定义了基于HTTP链路建立WebSocket信道的标准流程。

客户端通过发送如下HTTP Request告诉服务器需要建立一个WebSocket长链接信道：

```

```



