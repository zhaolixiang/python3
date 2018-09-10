### 同步与异步I/O对比

众所周知，CPU的运行效率高于磁盘的存储，也高于网络请求，这就导致CPU对数据的处理和数据的存储或者网络请求（I/O操作）步伐不一致，此时可以选择I/O操作同步或者异步。

同步I/O操作，导致进程阻塞，直到I/O操作完成;

异步I/O操作，不会导致请求进程阻塞。

### Tornado同步I/O的简单代码实例：

代码：

```
#导入Tornado的HTTP客户端
from tornado.httpclient import HTTPClient

def synchronous_visit():
    http_client=HTTPClient()
    #阻塞，知道对网址访问完成
    respone=http_client.fetch("http://www.baidu.com")
    print(respone.body)
synchronous_visit()
```

HTTPClient是Tornato的同步访问HTTP客户端。上述代码中的synchronous\_visit\(\)函数使用了典型的同步I/O操作来访问网址，该函数的执行时间取决于网络速度、对方服务器的响应速度，只有当访问完全结束并获取结果后，该函数才能执行完成。

### Tornado异步I/O的简单代码实例：

```
from tornado.httpclient import AsyncHTTPClient
def handle_response(response):
    print(response.body)

def asyncronous_visit():
    http_client=AsyncHTTPClient()
    http_client.fetch("http://www.baoidu.com",callback=handle_response)

asyncronous_visit()
```

AsyncHTTPClient是Tornado的异步访问HTTP客户端。在上述代码中的asynchronous\_visit\(\)函数中使用了AsyncHTTPClient对第三方网站进行异步访问，`http_client.fetch()`函数会在调用后立刻返回而无需等待实际访问的完成，从而导致`asynchronous`_`visit()`也会立刻执行完成。当对网址的访问实际完成后，AsyncHTTPClient会调用callback参数指定的函数，可以在这个函数中处理访问结果。

