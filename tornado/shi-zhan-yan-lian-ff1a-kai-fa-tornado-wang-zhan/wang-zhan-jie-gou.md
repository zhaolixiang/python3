### 实例：HelloWorld

```
import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello World")

def make_app():
    return tornado.web.Application([
        (r"/",MainHandler),

    ])

def main():
    app=make_app()
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()

if __name__=="__main__":
    main()
```

浏览器输入链接：[http://localhost:8888](http://localhost:8888)

页面显示：

```
Hello World
```

下面逐行解析上面的代码做了些什么：

1. 首先通过import语句引入tornado包中的ioloop和web类。这两个类是Tornado程序的基础。
2. 实现一个web.RequestHandler子类，重载其中的get\(\)函数，该函数负责相应定位到该RequestHandler的HTTP GET请求的处理。本实例通过self.write\(\)函数输出『Hello world』。
3. 定义了make\_app\(\)函数，该函数返回一个web.Application对象。



