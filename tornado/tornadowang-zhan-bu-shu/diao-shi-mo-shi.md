到目前为止，本章通过如下方式启动tornado程序：

```
def make_app():
    return tornado.web.Application([
        #此处写入映射
    ])
def main():
    app=make_app()  #建立Application对象
    app.listen(8888) #设置监听端口
    IOLoop.current().start()  @启动IOLoop

if __name__ == '__main__':
    main()
    
```



