Tornado定义了tornado.websocket.WebSocketHandler类用于处理WebSocket链接的请求，应用开发者应该继承该类并实现其中的open\(\)、on\_message\(\)、on\_close\(\)函数。

* WebSocketHandler.open\(\)函数：在一个新的WebSocket链接建立时，Tornado框架会调用此函数。在本函数中，开发者可以和在get\(\)、post\(\)等函数中一样用get\_argument\(\)函数获取客户端提交的参数，以及用get\_secure\_cookie/set\_secure\_cookir等操作Cookie等。
* WebSocketHandler.on\_message\(message\)函数：建立WebSocket链接后，当收到来自客户端的消息时，Tornado框架会调用本函数。通常，这是服务器端WebSocket编程的核心函数，通过解析收到的消息做出相应的处理。
* WebSocketHandler.on\__close\(\)函数：当WebSocket链接被关闭时，Tornado框架会调用本函数。在本函数中，可以通过访问self.close\_code和self.close\_reason查询关闭的原因。_

除了这三个Tornado框架自动调用的入口函数，WebSocketHandler还提供了两个开发者主动操作WebSocket函数。

* WebSocketHandler.write\_message\(message,binary=False\)函数：用于向与本链接相对于的客户端写入消息。

* WebSocketHandler.close\(code=None,reason=None\)



