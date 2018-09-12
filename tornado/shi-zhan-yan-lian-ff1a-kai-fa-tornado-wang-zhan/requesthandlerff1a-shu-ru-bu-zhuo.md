输入捕捉是指在RequestHandler中用于获取客户端输入的工具函数和属性。比如获取URL参数、Post提交参数等。

##### 1、get\_argument\(name\)、get\_arguments\(name\)

 RequestHandler.get_argument(name)与RequestHandler.get_arguments(name)都是返回给定参数的值。get_argument是获取单个值,而get_arguments在参数存在多个值得情况下使用，返回多个值的列表。
注意：使用这两个方法获取的事URL中查询的参数与POST提交的参数的参数合集。

#####2、get_query_argument(name)、get_query_arguments(name)
 功能与上面两个方法类似，唯一区别是这两个方法仅仅从URL中查询参数。
#####3、get_body_argument(name)、get_body_arguments(name)
功能尚与上面四个方法类似，唯一区别是这两个方法仅仅从POST提交的参数中查询。
>提示：实际开发中一般会使用get_argument、get_arguments这两个方法，因为他们会包含其他方法的查询结果。
#####4、get_cookie(name,default=None)
根据Cookie名称获取Cookie的值
#####5、 RequestHandler.request
返回tornado.httputil.HTTPServerRequest对象实例的属性，通过该对象可以获取关于HTTP请求的一切信息，比如：


```

```

