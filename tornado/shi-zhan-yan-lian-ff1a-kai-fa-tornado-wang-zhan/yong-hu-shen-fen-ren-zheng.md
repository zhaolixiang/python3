在Tornado的RequestHandler类中有一个current\_user属性用于保存当前请求的用户名。RequestHandler.get\_current\_user的默认值是None，在get\(\)、post\(\)等处理函数中可以随时读取该属性以获取当前的用户名。RequestHandler.current\_user是一个只读属性，所以如果想要设置该属性值，需要重载RequestHandler.get\_current\_user\(\)函数以设置该属性值。

##### 实例：使用RequestHandler.current\_user属性及RequestHandler.get\_current\_user\(\)方法来实现用户身份控制。

代码：

```

```



