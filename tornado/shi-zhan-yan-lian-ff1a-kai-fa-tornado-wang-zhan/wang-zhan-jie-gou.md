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



