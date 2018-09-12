Tornado有两种方式可改变同步的处理流程：

* 异步化：针对RequestHandler的处理函数使用@tornado.web.asynchronous修饰器，将默认的同步机制改为异步机制。该方法已经过期。
* 协程化：针对RequestHandler的处理函数使用@tornado.gen.coroutine修饰器，将默认的同步机制改为协程机制。

### 1、异步化

该方法已经过期，不再赘述，直接使用@tornado.gen.coroutine代替。

### 2、协程化

Tornado协程结合了同步处理和异步处理的有点，使得代码即清晰易懂，又能够适应海量客户端的高并发请求。

代码：

```
import tornado.web
import tornado.httpclient
from tornado.web import Application
import tornado.ioloop
class MainHandler(tornado.web.RequestHandler):


    @tornado.gen.coroutine
    def get(self):
        http=tornado.httpclient.AsyncHTTPClient()
        response=yield http.fetch("http://www.baidu.com")
        self.write(response.body)

if __name__ == '__main__':
    app=Application([
        ("/",MainHandler)
    ])
    app.listen("8888")
    tornado.ioloop.IOLoop.current().start()
```

协程化的关键技术点如下：

* 用tornado.gen.coroutine装饰MainHandler的get\(\)、post\(\)等处理函数。
* 使用异步对象处理耗时操作，比如本例的AsyncHTTPClient。
* 调用yield关键字获取异步对象的处理结果。



