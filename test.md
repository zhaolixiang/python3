### 继承BaseManager类，实现自定义对象的共享

> 如果想要共享用户自定义类的实例，则必须创建自定义管理器对象，为此，需要创建一个继承自BaseManager类的类，
>
> 其中BaseManager类定义在multiprocessing.managers子模块中。

* ##### 语法：

```
managers.BaseManager(address,authkey):用户自定对象创建管理器服务器的基类。
address是一个可选的元组（hostname,port），用于指定服务器的网络地址。如果忽略此参数，操作系统将简单
的分配一个对应于某些空闲端口号的地址。
authkey是一个字符串，用于对连接到服务器的客户端身份进行验证。如果忽略此参数，将是哟个current_process().authkey的值。
```

假设myclass是一个继承自BaseManager的类，可以使用以下类方法来创建用于返回代理给共享对象的方法。

```
myclass.register(typeid,callable,proxytype,exposed,method_to_typeid,create_method):使用管理器类注册一种新的数据类型。

typeid:一个字符串，用于命名特殊类型的共享对象。这个字符串应该是有效的Python标识符。

callable:创建或返回要共享的实例的可调用对象。

proxytype:一个类，负责提供客户端中药使用的代理对象的实现，通常这些类是默认生成的，因此一般设置为None。
exposed:共享对象上方法名的序列，它将会公开给代理对象，如果省略此参数，将使用proxytype_exposed的值，
如果proxytype_expose未定义，序列将包含所有的公共方法（所有不以下划线_开头的可调用方法）。

method_to_typeid:从方法名称到类型IDS的映射，这里的IDS用于指定哪些方法应该使用代理对象返回他们的结果，
如果某个方法在这个映射中找不到，将复制和返回它的返回值，如果method_to_typeid为None，将使用proxytype_method_to_typeid_的值。

create_method:一个布尔标志，用于指定是否在mgclass中创建名为typeid的方法，默认值为True。


```

从BaseManager类派生的管理器的实例m必须手动启动才能运行。

* ##### 属性于常用方法

| 属性或者方法 | 解释说明 |
| :--- | :--- |
| m.address | 元组\(hostname,port\)，代表管理器服务器正在使用的地址。 |
| m.connect\(\) | 连接带远程管理器对象，该对象的地址在BaseManager构造函数中支出。 |
| m.serve\_forever\(\) | 在当前进程中运行管理器服务器。 |
| m.shutdown\(\) | 关闭由start\(\)启动的管理器服务器。 |
| m.start\(\) | 启动一个单的子进程，并在该子进程中启动管理器服务器。 |



##### 



