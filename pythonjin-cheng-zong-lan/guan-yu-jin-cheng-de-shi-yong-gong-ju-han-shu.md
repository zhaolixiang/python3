# 9、关于进程的实用工具函数

| 函数 | 解析 |
| :--- | :--- |
| activite\_children\(\) | 返回所有活动子进程的Process对象组成的列表 |
| cpu\_count\(\) | 返回系统上的cpu数量，如果能够确定的话 |
| current\_process\(\) | 返回当前进程的Process对象 |
| freeze\_support\(\) | 在实用各种打包工具\(如py2exe\)进行冻结的应用程序中，次函数应该作为主程序的首行。实用此函数可以防止与在冻结的应用程序中启动子进程相关的运行错误。 |
| get\_logger\(\) | 返回与多进程处理模块相关的日制记录对象，如果它不存在则创建，返回的记录器不会把消息传播给跟记录器，级别为logging.NOTSET，而且会将所有日制消息打印到标准错误上。 |
| set\_executable\(executable\) | 设置用于执行子进程的Python可执行程序的名称。这个函数只定义在Windows上。 |



