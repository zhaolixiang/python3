Tornado定义了tornado.websocket.WebSocketHandler类用于处理WebSocket链接的请求，应用开发者应该继承该类并实现其中的open\(\)、on\_message\(\)、on\_close\(\)函数。

* WebSocketHandler.open\(\)函数：在一个新的WebSocket链接建立时，Tornado框架会调用此函数。在本函数中，开发者可以和在get\(\)、post\(\)等函数中一样用get\_argument\(\)函数获取客户端提交的参数，以及用get\_secure\_cookie/set\_secure\_cookir等操作Cookie等。
* WebSocketHandler.on\_message\(message\)函数：建立WebSocket链接后，当收到来自客户端的消息时，Tornado框架会调用本函数。通常，这是服务器端WebSocket编程的核心函数，通过解析收到的消息做出相应的处理。
* WebSocketHandler.on\__close\(\)函数：当WebSocket链接被关闭时，Tornado框架会调用本函数。在本函数中，可以通过访问self.close\_code和self.close\_reason查询关闭的原因。_

除了这三个Tornado框架自动调用的入口函数，WebSocketHandler还提供了两个开发者主动操作WebSocket函数。

* WebSocketHandler.write\_message\(message,binary=False\)函数：用于向与本链接相对于的客户端写入消息。

* WebSocketHandler.close\(code=None,reason=None\)函数：主动关闭WebSocket链接。其中的code和reason用于告诉客户端链接被关闭的原因。参数code必须是一个数值，而reason是一个字符串。

下面是持续为客户端推送时间消息的Tornado WebSocket程序：

```
import tornado.ioloop
import tornado.web
import tornado.websocket
from tornado.options import define,options,parse_command_line

define("port",default=8888,help="run on the given post",type=int)

clients=dict()#客户端Session字典

class IndexHandler(tornado.web.RequestHandler):
    @tornado.web.asynchronous
    def get(self):
        self.render("index.html")

class MyWebSocketHandler(tornado.websocket.WebSocketHandler):
    def open(self, *args, **kwargs): #有新链接时被调用
        self.id=self.get_argument("Id")
        self.stream.set_nodelay(True)
        clients[self.id]={"id":self.id,"object":self}#保存Session到clients字典中

    def on_message(self, message):#收到消息时被调用
        print("Client %s received a message:%s"%(self.id,message))

    def on_close(self): #关闭链接时被调用
        if self.id in clients:
            del clients[self.id]
            print("Client %s is closed"%(self.id))

    def check_origin(self, origin):
        return True
app=tornado.web.Application([
    (r'/',IndexHandler),
    (r'/websocket',MyWebSocketHandler),
])


import threading
import time
#启动单独的线程运行此函数，每隔1秒向所有的客户端推送当前时间
def sendTime():
    import datetime
    while True:
        for key in clients.keys():
            msg=str(datetime.datetime.now())
            clients[key][object].write_mesage(msg)
            print("write to client %s:%s"%(key,msg))
        time.sleep(1)

if __name__ == '__main__':
    #启动推送时间线程
    threading.Thread(target=sendTime()).start()
    parse_command_line()
    app.listen(options.port)
    #挂起运行
    tornado.ioloop.IOLoop.instance().start()





```



