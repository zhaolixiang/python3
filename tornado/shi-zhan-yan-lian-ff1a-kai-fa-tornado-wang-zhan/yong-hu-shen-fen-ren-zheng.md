在Tornado的RequestHandler类中有一个current\_user属性用于保存当前请求的用户名。RequestHandler.get\_current\_user的默认值是None，在get\(\)、post\(\)等处理函数中可以随时读取该属性以获取当前的用户名。RequestHandler.current\_user是一个只读属性，所以如果想要设置该属性值，需要重载RequestHandler.get\_current\_user\(\)函数以设置该属性值。

##### 实例：使用RequestHandler.current\_user属性及RequestHandler.get\_current\_user\(\)方法来实现用户身份控制。

代码：

```

```

本例演示了一个完整的身份认证编程框架，整体构思如下：

* 用全局字典dict\_sessions保存已经登录的用户信息，为了简单些，本例只保存了【回话ID：用户名】的键值对。
* 定义公共基类BaseHandler，该类继承自tornado.web.RequestHandler，用于定义本网站所有处理器的公共属性和行为。重载它的get_current_user()函数，其在访问RequestHandler.current_user属性时自动被Tornado调用。该函数首先用get_secure_cookie()获得本次访问的回话ID,然后利用该ID从dict_sessions中获得用户名并且返回。



