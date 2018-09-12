Cookie是很多网站为了辨别用户的身份而存储在用户本地终端\(Client Side\)d的数据，在Tornado中使用RequestHandler.get\_cookie\(\)、RequestHandler.set\_cookie\(\)可以方便地对Cookie进行读写。

###  实例：Cookie的简单读写

```
import tornado.web

session_id = 1
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        global session_id
        if not self.get_cookie("session"):
            self.set_cookie("session",str(session_id))
            session_id+=1
            self.write("设置新的session")
        else:
            self.write("已经具有session")

if __name__ == '__main__':
    app=tornado.web.Application([
        ("/",MainHandler)
    ])
    app.listen("8888")
    tornado.ioloop.IOLoop.current().start()
```

本例中用get\_cookie\(\)函数判断Cookie名【session】是否存在，如果不存在则为其赋予新的session\_id.

> 在实际应用中，Cookie经常像本例这样用于保存session信息。  
> 因为Cookie总是被保存在客户端，所以如何保存其不被篡改是服务器端程序必须解决的问题。  
> Tornado为Cookie提供了信息加密机制，使得客户端无法随意解析和修改Cookie的键值。

### 实例：安全的Cookie



