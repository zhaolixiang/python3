输入捕捉是指在RequestHandler中用于获取客户端输入的工具函数和属性。比如获取URL参数、Post提交参数等。

##### 1、get\_argument\(name\)、get\_arguments\(name\)

RequestHandler.get\_argument\(name\)与RequestHandler.get\_arguments\(name\)都是返回给定参数的值。get\_argument是获取单个值,而get\_arguments在参数存在多个值得情况下使用，返回多个值的列表。  
注意：使用这两个方法获取的事URL中查询的参数与POST提交的参数的参数合集。

##### 2、get\_query\_argument\(name\)、get\_query\_arguments\(name\)

功能与上面两个方法类似，唯一区别是这两个方法仅仅从URL中查询参数。

##### 3、get\_body\_argument\(name\)、get\_body\_arguments\(name\)

功能尚与上面四个方法类似，唯一区别是这两个方法仅仅从POST提交的参数中查询。

> 提示：实际开发中一般会使用get\_argument、get\_arguments这两个方法，因为他们会包含其他方法的查询结果。

##### 4、get\_cookie\(name,default=None\)

根据Cookie名称获取Cookie的值

##### 5、 RequestHandler.request

返回tornado.httputil.HTTPServerRequest对象实例的属性，通过该对象可以获取关于HTTP请求的一切信息，比如：

```
from tornado.web import Application, RequestHandler
import tornado.ioloop


class DetailHandler(RequestHandler):
    def get(self):
        ip = self.request.remote_ip  # 获取客户端的IP地址
        host = self.request.host  # 获取请求的主机地址
        result="ip地址为%s,host为%s"%(ip,host)
        return self.write(result)


if __name__ == '__main__':
    app = Application([
        ("/request", DetailHandler)
    ])
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()
```

浏览器输入：http://localhost:8888/request

页面显示：

```
ip地址为::1,host为localhost:8888
```

常用的httputil.HTTPServerRequest对象属性如下表：

| 属性名 | 说明 |
| :--- | :--- |
| method | HTTP请求方法，例如：GET、POST |
| uri | 客户端请求的uri的完整内容。 |
| path | uri路径名，即不包含查询字符串 |
| query | uri中的查询字符串 |
| version | 客户端发送请求时使用的HTTP版本，例如：HTTP/1.1 |
| headers |  |
| body |  |
| remote\_ip |  |
| protocol |  |
| host |  |
| arguments |  |
| cookies |  |



