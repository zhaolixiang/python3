Tornado定义了tornado.websocket.WebSocketHandler类用于处理WebSocket链接的请求，应用开发者应该继承该类并实现其中的open\(\)、on\_message\(\)、on\_close\(\)函数。

* WebSocketHandler.open\(\)函数：在一个新的WebSocket链接建立时，Tornado框架会调用此函数。在本函数中，开发者可以和在get()、post()等函数中一样用get_argument\(\)函数获取客户端提交的参数，以及用get_secure_cookie/set_secure_cookir等操作Cookie等。




