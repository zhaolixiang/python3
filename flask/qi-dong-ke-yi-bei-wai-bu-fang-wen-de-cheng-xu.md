可以被外部访问的启动方式

```
flask run --host=0.0.0.0
```

查看所有路由

```
flask routes
```

自定义模板的全局函数：

1、使用app.template\_global\(name=自定义函数名默认为函数名\)

2、调用app.add_template_global\(\)方法

