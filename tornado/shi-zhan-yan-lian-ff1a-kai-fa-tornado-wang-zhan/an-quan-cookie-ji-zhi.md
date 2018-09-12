Cookie是很多网站为了辨别用户的身份而存储在用户本地终端\(Client Side\)d的数据，在Tornado中使用RequestHandler.get\_cookie\(\)、RequestHandler.set\_cookie\(\)可以方便地对Cookie进行读写。  
实例：

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

本例中用get_cookie\(\)函数判断Cookie名【session】是否存在，如果不存在则为其赋予新的session_id.

