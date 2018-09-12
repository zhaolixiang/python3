输出响应函数是指一组为客户端生成处理结果的工具函数，开发者调用它们以控制URL的处理结果。常用的输出相应函数如下：

##### 1、RequestHandler.set_status(status_code,reason=None)
设置HTTP Response中的返回码，如果有描述性的语句，则可以赋值给reason参数。
##### 2、RequestHandler.set_header(name,value)
以键值对的方式设置HTTP Response中的HTTP头参数，使用set_header配置的Header值将覆盖之前配置的Header。




