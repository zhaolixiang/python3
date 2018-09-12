Tornado有两种方式可改变同步的处理流程：

* 异步化：针对RequestHandler的处理函数使用@tornado.web.asynchronous修饰器，将默认的同步机制改为异步机制。该方法已经过期。
* 协程化：针对RequestHandler的处理函数使用@tornado.gen.coroutine修饰器，将默认的同步机制改为协程机制。

### 1、异步化

该方法已经过期，不再赘述，直接使用@tornado.gen.coroutine代替。

### 2、协程化



