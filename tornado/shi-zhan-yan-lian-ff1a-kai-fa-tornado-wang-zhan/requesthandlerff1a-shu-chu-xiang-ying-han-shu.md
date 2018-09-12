输出响应函数是指一组为客户端生成处理结果的工具函数，开发者调用它们以控制URL的处理结果。常用的输出相应函数如下：

##### 1、RequestHandler.set_status(status_code,reason=None)
设置HTTP Response中的返回码，如果有描述性的语句，则可以赋值给reason参数。
##### 2、RequestHandler.set_header(name,value)
以键值对的方式设置HTTP Response中的HTTP头参数，使用set_header配置的Header值将覆盖之前配置的Header。
##### 3、RequestHandler.add_header(name,value)
以键值对的方式设置HTTP Response中的HTTP头参数。与set_header不同的是add_header配置的Header值将不会覆盖之前配置的Header。
##### 4、RequestHandler.write(chunk)
将给定的块作为HTTP Body发送客户端。在一般情况下，用本函数输出字符串给客户端。
如果给定的块是一个字典，则会将这个块以JSON格式发送给客户端，同时将HTTP Header中的Content_Type设置为application/json.

##### 5、RequestHandler.finish(chunk=None)
本方法通知Tornado.Response的生成工作已完成，chunk参数是需要传递给客户端的HTTP body。调用finish()后，Tornado将向客户端发送HTTP Response。
本方法适用于对RequestHandler的异步请求处理，在同步或协程访问处理的函数中，无须调用finish()函数。

##### 6、RequestHandler.render(template_name,**kwargs)
用给定的参数渲染模块，可以在本函数中传入模板文件名称和模板参数。
实例
```
import tornado.web
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        items=["Python","C++","Java"]
        #第一个参数是模板名称，后面是模板参数
        self.render("template.html",title="Tornado Template",items=items)
```
##### 7、RequestHandler.redirect(url,permanent=False,status=None)
进行页面重定向。在RequestHandler处理过程中，可以随时调用redirect()函数进行页面重定向。
##### 8、RequestHandler.clear()
清空所有在本次请求中之前写入的Header和Body内容。
##### 9、RequestHandler.set_cookie(name,value)
按键值对设置Response中的Cookie的值
##### 10、RequestHandler.clear_all_cookies(path="/",domain=None)
清空本次请求中的所有Cookie




