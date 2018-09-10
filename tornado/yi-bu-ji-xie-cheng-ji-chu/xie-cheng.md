使用Tornado协程可以开发出类似同步代码的异步行为。同时，因为协程本身不使用线程，所以减少了线程上下文切换的开销，是一种高效的开发模式。

1、编写协程函数

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



