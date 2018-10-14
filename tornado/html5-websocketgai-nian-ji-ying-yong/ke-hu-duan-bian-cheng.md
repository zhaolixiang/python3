由于WebSocket是HTML5的标准之一，所以主流浏览器的Web客户端编程语音JavaScript已经支持WebSocket的客户端编程。

客户端编程围绕着WebSocket对象展开，在JavaScript中可以通过如下代码初始化WebSocket对象：

```
var Socket=new WebSocket(url);
```

在代码中只需给WebSocket构造函数传入服务器的URL地址，比如[http://mysite.com/point](http://mysite.com/point).

可以为该对象的如下事件指定处理函数以相应它们：

* WebSocket.onopen：此事件发生在WebSocket链接建立时。
* WebSocket.onmessage：此事件发生在收到了来自服务器的消息时。
* WebSocket.onerror：此事件发生在通信过程中有任何错误时。

* WebSocket.onclose：此事件发生在服务器的链接关闭时。

除了这些事件处理函数，还可以通过WebSocket对象的两个方法进行主动操作：

* WebSocket.send\(data\)：向服务器发送消息。
* WebSocket.close\(\)：主动关闭现有链接。

客户端WebSocket编程实例程序如下：index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>WebSocket</title>
</head>
<body>
<a href="javascript:WebSocketTest()">运行WebSocket</a>
<div id="messages" style="height: 200px;background: black;color:white"></div>

</body>
<script type="text/javascript">
    var messageContainer=document.getElementById("messages");
    function WebSocketTest() {
        if("WebSocket" in window){
            messageContainer.innerHTML="浏览器支持WebSocket";
            var ws=new WebSocket("ws://localhost:8888/websocket?Id=12345");
            ws.onopen=function () {
                ws.send("onopen")
            };
            ws.onmessage=function (evt) {
                var received_msg=evt.data;
                messageContainer.innerHTML=messageContainer.innerHTML+"<br/>收到的信息："+received_msg;
            }
            ws.onclose=function () {
                messageContainer.innerHTML=messageContainer.innerHTML+"<br/> 连接关闭了";
            }


        }else{
            messageContainer.innerHTML="浏览器不支持WebSocket"
        }
    }
</script>
</html>
```

对上述代码解析如下：

* 客户端页面主体是有两部分组成：一个Run WebSocket链接用于让用户启动WebSocket；另一个id=message的&lt;div&gt;标签用于显示服务器端的消息。
* 使用JavaScript语句if\("WebSocket"  in  window\)可以判断当前浏览器是否支持WebSocket对象。
* 如何浏览器支持WebSocket对象，则定义实例ws链接到服务器的WebSocket地址，并传入自己的标识符参数。然后通过js语法定义事件：onopen、onmessage、onclose的处理函数。除了在onopen事件中客户端向服务器用WebSocket.send\(\)函数发送了消息，其余事件均只将事件结果显示在页面&lt;div&gt;标签中。

运行效果如下：





