输入捕捉是指在RequestHandler中用于获取客户端输入的工具函数和属性。比如获取URL参数、Post提交参数等。

##### 1、get\_argument\(name\)、get\_arguments\(name\)

>RequestHandler.get_argument(name)与RequestHandler.get_arguments(name)都是返回给定参数的值。get_argument是获取单个值,而get_aeguments在参数存在多个值得情况下使用，返回多个值的列表。

