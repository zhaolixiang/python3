在Tornado的RequestHandler类中有一个current_user属性用于保存当前请求的用户名。RequestHandler.get_current_user的默认值是None，在get()、post()等处理函数中可以随时读取该属性以获取当前的用户名。RequestHandler.current_user是一个只读属性，所以如果想要设置该属性值，需要重载RequestHandler.get_current_user()函数以设置该属性值。
##### 实例：使用RequestHandler.current_user属性及RequestHandler.get_current_user()方法来实现用户身份控制。
代码：


```

```



