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

* 在本身是协程的函数内通过yield关键字调用。
* 在IOLoop尚未启动时，通过IOLoop的run\_sync\(\)函数调用。
* 在IOLoop已经启动时，通过IOLoop的spawn\_callback\(\)函数调用。

##### 实例：通过协程函数调用协程函数

代码：

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

@gen.coroutine
def outer_coroutine():
    print("start call coroutine_visit")
    yield coroutine_visit()
    print("end call coroutine_cisit")
```

本例中outer_coroutine\(\)和coroutine\_visit\(\)都是协程函数，所以他们之间可以通过yield关键字调用。_

##### 实例：IOLoo尚未启动时，通过IOLoop的run\_sync\(\)函数调用。

> IOLoop是Tornado的主事件循环对象，Tornado程序通过它监听外部客户端的访问请求，并执行相应操作。

代码：

```
#用协程技术开发网页访问功能
from tornado import  gen #引入协程库gen
from tornado.httpclient import AsyncHTTPClient
from tornado.ioloop import IOLoop  #引入IOLoop对象

#使用gen.coroutine修饰器
@gen.coroutine
def coroutine_visit():
    http_client=AsyncHTTPClient()
    response=yield http_client.fetch("http://www.baidu.com")
    print(response.body)

def func_normal():
    print("start call coroutine_visit")
    IOLoop.current().run_sync(lambda :coroutine_visit())
    print("end call coroutine_visit")
```

> 当程序尚未进入IOLoop的running状态时，可以通过run\_sync\(\)函数调用协程函数。
>
> ⚠️注意：run\_sync\(\)函数将阻塞当前函数的调用，直到被调用的协程执行完成。
>
> 事实上，Tornado要求协程函数在IOLoop的running状态种才能被调用，只不过run\_sync函数自动完成了启动、停止IOLoop的操作步骤，他的实现逻辑是：
>
> 【启动IOLoop】》【调用被lambda封装的协程函数】》【停止IOLoop】

##### 实例：在IOLoop启动时，通过spawn\_callback\(\)函数调用

代码：

```
#用协程技术开发网页访问功能
from tornado import  gen #引入协程库gen
from tornado.httpclient import AsyncHTTPClient
from tornado.ioloop import IOLoop  #引入IOLoop对象

#使用gen.coroutine修饰器
@gen.coroutine
def coroutine_visit():
    http_client=AsyncHTTPClient()
    response=yield http_client.fetch("http://www.baidu.com")
    print(response.body)

def func_normal():
    print("start call coroutine_visit")
    IOLoop.current().spawn_callback(coroutine_visit)
    print("end call coroutine_visit")
```

> spawn\_callback\(\)函数将不会等待被调用协程执行完成，所有上下两条打印语句将马上完成，而coroutine\_\_visit本身将会由IOLoop在合适的时机进行调用。
>
> ⚠️注意：IOLoop的spawn\_callback\(\)函数没有为开发者提供获取协程函数调用返回值的方法，所以只能用span\_callback\(\)调用没有返回值的协程函数。

### 3、在协程中调用阻塞函数

> 在协程中直接调用阻塞函数会影响协程本身的性能，所以Tornado提供了在协程中利用线程池调度阻塞函数，从而不影响协程本身继续执行的方法。

代码实例：

```
from concurrent.futures import ThreadPoolExecutor
from tornado import gen

#定义线程池
thread_pool=ThreadPoolExecutor(2)

def mySleep(count):
    import time
    for x in range(count):
        time.sleep(1)

@gen.coroutine
def call_blocking():
    print("start")
    yield thread_pool.submit(mySleep,10)
    print("end")
```

代码中首先引用了concurrent.futures种的ThreadPoolExecutor类，实例化了一个由两个线程的线程池thread\_pool。在需要调用阻塞函数的协程call\_blocking种，使用thread\_pool.submit调用阻塞函数，并通过yield返回。这样便不会阻塞协程所在的线程的继续执行，也保证了阻塞函数前后代码的执行顺序。

### 4、在协程中等待多个异步调用

> 到目前为止，我们知道了协程中一个yield关键字等待一个异步调用的编程方法。其实，Tornado允许在协程中用一个yield关键字等待多个异步调用，只需要把这些调用以列表\(list\)或字典\(dictionary\)的方式传递给yield关键字即可。

##### 实例：使用列表方式传递多个异步调用

```
#使用列表方式传递多个异步调用
from tornado import gen  #引入协程库gen
from tornado.httpclient import AsyncHTTPClient

@gen.coroutine   #使用gen.coroutine修饰器
def coroutine_visit():
    http_client=AsyncHTTPClient()
    list_response=yield [
        http_client.fetch("http://www.baidu.com"),
        http_client.fetch("http://www.api.jiutouxiang.com")
    ]
    for response in list_response:
        print(response.body)
```

在代码中仍然使用@gen.coroutine装饰器定义协程，在需要yield的地方用列表传递若干个异步调用，只有在列表种的所有调用都执行完成后，yield才会返回并且继续执行。yield以列表方式返回调用结果。

##### 实例：用字典方式传递多个异步调用：

```
#使用列表方式传递多个异步调用
from tornado import gen  #引入协程库gen
from tornado.httpclient import AsyncHTTPClient

@gen.coroutine   #使用gen.coroutine修饰器
def coroutine_visit():
    http_client=AsyncHTTPClient()
    dict_response=yield {
       "baidu": http_client.fetch("http://www.baidu.com"),
        "9siliao":http_client.fetch("http://www.api.jiutouxiang.com")
    }
    print(dict_response["baidu"].body)
```



