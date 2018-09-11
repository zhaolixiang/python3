向web.Application对象传递的第1个参数URL路由映射列表的配置方式与Django类型，用正则字符串进行路由匹配。

> Tornado的路由字符串有两种，固定字符串路径和参数字符串路径

### 1、固定字串路径

> 固定字符串即是普通的字符串固定匹配，比如：

```
Handlers=[
("/",MainHandler),                  #只匹配跟路径
("/entry",EntryHandler)             #只匹配/entry
("/entry/2019",Entry2019Handler)    #只匹配/entry/2019
]
```

### 2、参数字符路径

参数子串可以将具备一定模式的路径映射到同一个RequestHandler中处理，其中路径中的参数部分用小括号"\(\)"标识。

实例：参数路径

```

```



