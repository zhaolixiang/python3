由于WebSocket是HTML5的标准之一，所以主流浏览器的Web客户端编程语音JavaScript已经支持WebSocket的客户端编程。

客户端编程围绕着WebSocket对象展开，在JavaScript中可以通过如下代码初始化WebSocket对象：

```
var Socket=new WebSocket(url);
```

在代码中只需给WebSocket构造函数传入服务器的URL地址，比如http://mysite.com/point.

可以为该对象的如下事件指定处理函数以相应它们：

* WebSocket.onopen：此事件发生在WebSocket链接建立时。
* WebSocket.onmessage：此事件发生在收到了来自服务器的消息时。
* WebSocket.onerror：此事件发生在通信过程中有任何错误时。

* WebSocket.onclose：此事件发生在服务器的链接关闭时。





