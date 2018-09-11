使用Tornado协程可以开发出类似同步代码的异步行为。同时，因为协程本身不使用线程，所以减少了线程上下文切换的开销，是一种高效的开发模式。

### 1、编写协程函数

实例：用协程技术开发网页访问功能

```
#用协程技术开发网页访问功能
from tornado import  gen #引入协程库gen
from tornado.httpclient import AsyncHTTPClient
import time

#使用gen.coroutine修饰器
@gen.coroutine
def coroutine_visit():
    http_client=AsyncHTTPClient()
    response=yield http_client.fetch("http://www.baidu.com")
    print(response.body)
```

本例中任然使用了异步客户端AsyncHTTPClient进行页面访问，装饰器@gen.coroutine声明这是一个协程函数，由于yield关键字的作用，使得代码中不用再编写回调函数用于处理访问结果，而可以直接在yield语句的后面编写结果处理语句。

### 2、调用协程函数
由于Tornado协程基于Python的yield关键字实现，所以不能像普通函数那样直接调用。
协程函数可以通过以下三张方式调用：






