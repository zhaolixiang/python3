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

通过这种方式启动的程序一旦出错，则只能通过Windows任务管理器或Linux命令行Kill掉Python进行。因为调试需要频繁地进行：启动→差错→停止→排错→重启...的迭代流程，所以这样简单的方法并不利于程序调试，本节学习如何简化调试流程。

# 1、自动加载

通过向Application实例传入参数debug=True，可以将程序以调试模式启动，例如：

```

```



