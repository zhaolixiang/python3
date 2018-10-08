在Tornado的RequestHandler类中有一个current\_user属性用于保存当前请求的用户名。RequestHandler.get\_current\_user的默认值是None，在get\(\)、post\(\)等处理函数中可以随时读取该属性以获取当前的用户名。RequestHandler.current\_user是一个只读属性，所以如果想要设置该属性值，需要重载RequestHandler.get\_current\_user\(\)函数以设置该属性值。

##### 实例：使用RequestHandler.current\_user属性及RequestHandler.get\_current\_user\(\)方法来实现用户身份控制。

代码：

```
import tornado.web
import tornado.ioloop
import uuid  #UUID 生成库

dict_sessions={}  #保存所有登录的Session

class BaseHandler(tornado.web.RequestHandler):  #公共基类
    #写入current_user的函数
    def get_current_user(self):
        session_id=self.get_secure_cookie("session_id")
        return dict_sessions.get(session_id)

class MainHandler(BaseHandler):
    @tornado.web.authenticated    #需要身份认证才能访问的处理器
    def get(self):
        name=tornado.escape.xhtml_escape(self.current_user)
        self.write("Hello,"+name)

class LoginHandler(BaseHandler):
    def get(self):   #登陆页面
        self.write('<html><>body'
                   '<form action="/login" method="post">'
                   'Name:<input type="text" name="name">'
                   '<input type="submit" value="Sign in">'
                   '</form></body></html>')
    def post(self):  #验证是否运行登陆
        if len(self.get_argument("name"))<3:
            self.redirect("/login")
        session_id=str(uuid.uuid1())
        dict_sessions[session_id]=self.get_argument("name")
        self.set_secure_cookie("session_id",session_id)
        self.redirect("/")
setting={
    "cookie_secret":"SECRET_DONT_LEAK", #Cookie加密秘钥
    "login_url":"/login"  #定义登陆页面
}
application=tornado.web.Application([
    (r"/",MainHandler),        #URL映射定义
    (r"/login",LoginHandler)
],**setting)

def main():
    application.listen(8888)
    tornado.ioloop.IOLoop.current().start()     #挂起监听

if __name__ == '__main__':
    main()

```

本例演示了一个完整的身份认证编程框架，整体构思如下：

* 用全局字典dict\_sessions保存已经登录的用户信息，为了简单些，本例只保存了【回话ID：用户名】的键值对。
* 定义公共基类BaseHandler，该类继承自tornado.web.RequestHandler，用于定义本网站所有处理器的公共属性和行为。重载它的get\_current\_user\(\)函数，其在访问RequestHandler.current\_user属性时自动被Tornado调用。该函数首先用get\_secure\_cookie\(\)获得本次访问的回话ID,然后利用该ID从dict\_sessions中获得用户名并且返回。
* MainHandler类是一个要求用户经过身份认证才能访问的处理器实例。该处理器中的处理函数get\(\)使用了装饰器tornado.web.authenticated，具有该装饰器的处理函数在执行之前根据current\_user是否已经被赋值来判断用户的身份认证情况，如果已经被赋值则可以进行正常逻辑，否则自动重定向到网站的登录页面。
* LoginHandler类是登录页面处理器，其get\(\)函数用于渲染登录页面，post\(\)函数用于验证是否允许用户登陆。
* 在tornado.web.Application的初始化函数中通过login\_url参数给出网站的登陆页面地址。该地址被用于tornado.web.authenticated装饰器在发现用户尚未验证时重定向到一个URL。

> 注意：加入身份认证的所有页面处理器需要继承自BaseHandler类，而不是直接继承原来的tornado.web.RequestHandler类。

商用的身份认证还要完善更多的内容，比如加入密码验证机制、管理登陆超时、将用户信息保存到数据库等。

