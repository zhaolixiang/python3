需要子类继承并定义具体行为的安徽神女湖再RequestHandler中被称为接入点函数\(Entry point\),上面的Hello World实例中的get\(\)函数就是典型的接入点函数。

### 1、RequestHandler.initialize\(\)

该方法被子类重写，实现了RequestHandler子类实现的初始化过程。

可以为该函数传递参数（参数来源于配置URL映射的定义）。

##### 实例：

```
from tornado.web import RequestHandler,Application
import tornado.ioloop
import tornado.web

class ProfileHandler(RequestHandler):
    def initialize(self,database):
        self.database=database

    def get(self):
        return self.write(self.database)

    def post(self):
        pass

def make_app():
    return Application([
    (r"/test",ProfileHandler,dict(database="test.db",))
])

def main():
    app=make_app()
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()

if __name__=="__main__":
    main()
```

在浏览器上输入：[http://localhost:8888/test](http://localhost:8888/test)

页面显示：

```
test.db
```



