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
def make_app():
    return tornado.web.Application([
        #此处写入映射
    ],
    debug=True  #调试模式
    )
```

在这种模式下开发者可以获得如下便利：

* 自动加载：对项目中任何\*.py源文件的修改将导致程序自动重启并加载修改后的代码文件。这样极大地简化了开发者因为调试器需要频繁重启。
* 错误追溯：当RequestHandler；处理用户访问出现异常时，系统的错误信息调用栈将被推送到浏览器中，使得调试者可以马上查找错误的根源。
* 禁用模板缓存：在运营环境中模板缓存能提高效率，但在调试期间占用了更多的系统资源，所以将其禁用有利于开发者进行调试。

> 注意：在运营环境中不要开启Debug模式，这样会增加网站被攻击的危险。

# 2、Ctrl+C退出机制

在默认情况下Tornado的IOLoop不会响应Linux控制台的Ctrl+C命令，导致程序无法便捷地退出运行。

要响应Linux控制台的Ctrl+C命令，则可以在运行中捕获KeyboardInterrupt异常并调用IOLoop.stop\(\)函数：

```
def main():
    app=make_app()  #建立Application对象
    app.listen(8888) #设置监听端口
    try:
        #启动IOLoop
        tornado.ioloop.IOLoop.current().start()
    except KeyboardInterrupt:
        tornsfo.ioloop.IOLoop.current().stop()
        #此处执行资源回收工作
        print("Program exit!")

if __name__ == '__main__':
    main()
```

这也在控制台发送了Ctrl+C请求后，程序可有机会回收系统的其它资源并退出执行

