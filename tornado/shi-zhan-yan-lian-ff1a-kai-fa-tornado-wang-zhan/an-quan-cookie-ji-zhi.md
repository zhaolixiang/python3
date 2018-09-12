Cookie是很多网站为了辨别用户的身份而存储在用户本地终端\(Client Side\)d的数据，在Tornado中使用RequestHandler.get\_cookie\(\)、RequestHandler.set\_cookie\(\)可以方便地对Cookie进行读写。

### 实例：Cookie的简单读写

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

代码：

```
import tornado.web

session_id = 1
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        global session_id
        #get_secure_cookie代替get_cookie
        if not self.get_secure_cookie("session"):
            #set_secure_cookie代替set_cookie
            self.set_secure_cookie("session",str(session_id))
            session_id+=1
            self.write("设置新的session")
        else:
            self.write("已经具有session")

if __name__ == '__main__':
    app=tornado.web.Application([
        ("/",MainHandler)
    ],cookie_secret="JIA_MI_MI_YAO")
    app.listen("8888")
    tornado.ioloop.IOLoop.current().start()
```

对比上面的简单Cookie实例可以发现不同之处：

* 在tornado.web.Application对象初始化时赋予cookie\_secret参数，该参数值时一个字符串，用于保存本网站Cookie加密时的密钥。
* 在需要读取cookie的地方使用RequestHandler.get\_secure\_cookie代替原来的RequestHandler.get\_cookie调用。
* 在需要写入Cookie的地方用RequestHandler.set\_secure\_cookie替换原来的RequestHandler.set\_cookie调用，

这样，就不需要担心Cookie伪造的问题了，但是cookie\_secret参数值作为加密密钥，需要好好保护，不能泄露。

> 注意：加入身份认证的所有页面处理器需要继承自BaseHandler类，而不是直接继承原来的tornado.web.RequestHandler类。
>
> 商用的用户身份认证还需要完善更多的内容，例如：密码验证机制、管理登录超时、将用户信息保存到数据库等。



