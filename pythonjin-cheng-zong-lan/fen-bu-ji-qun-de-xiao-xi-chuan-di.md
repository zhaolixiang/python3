# 8、分布集群的消息传递

> 使用multiprocessing模块的程序不仅可以于运行在同一计算机的其它程序进行消息传递，还可以于位于不到计算机的远程系统程序进行消息传递。其中的multiprocessing.connection子模块包含该目的的函数和类：

connections.Client\(address,family,authenticate,authkey\)

```
连接到另一个进程，此进程必须已经正在监听地址address。
address:代表网络地址的元组(hostname,port),或者代表UNIX域套接字的文件名，或者代表
r'\\servername\pipe\pipename'形式的字符串，代表远程系统servername(本地计算机的servername为'.')上的一条Windows命名管道。

family:表示地址格式的字符串。一般是'AF_INET'、'AF_UNIX'、或'AF_PIPE'.如果省略此参数，将从address的格式退出它的值。

backlog:是一个整数，当address参数指定了一个网络连接时，对应于传递给套接字的listen()方法的值，backlog默认为1。

authenticate:一个布尔标志，指定是否使用摘要身份验证。
authkey:包含身份验证密钥的字符串，如果忽略此参数，将使用current_process().authkey的值。

此函数的返回值是Connection对象，管道中有讲过。
```

connections.Listener\(address,family,backlog,authenticate,authkey\)

```
实现了一台服务器，用于侦听和处理Client()函数发送的连接。
如果省略address参数，将选择默认地址，如果同时省略address和family两个参数，将选择本地系统上速度最快的可用通信模式。
```

Listener实例listener支持一下方法和属性。

| 属性或方法名 | 介绍 |
| :--- | :--- |
| listener.accept\(\) | 接受一个新连接，并返回一个Connetion对象。如果身份验证失败，将引发Authentication-Error异常 |
| listener.address | 侦听器正在使用的地址 |
| listener.close\(\) | 关闭侦听器正在使用的管道或套接字 |
| listener.last\_accepted | 接受的最后一个客户端的地址。 |



