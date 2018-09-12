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

### 2、RequestHandler.prepare\(\)、RequestHandler.on\_finish\(\)

prepare\(\)方法用于调用请求处理（get、post等）方法之前的初始化处理，通常用来做资源初始化操作。

on\_finish\(\)方法用于请求处理结束后的一些清理工作，通常用来清理对象占用的内存或者关闭数据库连接等工作。

### 3、HTTP Action处理函数

每个HTTP Action在RequestHandler中都以单独的函数进行分开处理：

* RequestHandler.get\(\*args,\*\*kwargs\)
* RequestHandler.post\(\*args,\*\*kwargs\)
* RequestHandler.head\(\*args,\*\*kwargs\)
* RequestHandler.delete\(\*args,\*\*kwargs\)
* RequestHandler.patch\(\*args,\*\*kwargs\)
* RequestHandler.put\(\*args,\*\*kwargs\)
* RequestHandler.options\(\*args,\*\*kwargs\)

每个处理函数都是HTTP Action的小写名字命名。



