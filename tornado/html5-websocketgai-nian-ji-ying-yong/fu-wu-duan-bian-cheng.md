Tornado定义了tornado.websocket.WebSocketHandler类用于处理WebSocket链接的请求，应用开发者应该继承该类并实现其中的open\(\)、on\_message\(\)、on\_close\(\)函数。

* WebSocketHandler.open\(\)函数：在一个新的WebSocket链接建立时，Tornado框架会调用此函数。在本函数中，开发者可以和在get\(\)、post\(\)等函数中一样用get\_argument\(\)函数获取客户端提交的参数，以及用get\_secure\_cookie/set\_secure\_cookir等操作Cookie等。
* WebSocketHandler.on\_message\(message\)函数：建立WebSocket链接后，当收到来自客户端的消息时



