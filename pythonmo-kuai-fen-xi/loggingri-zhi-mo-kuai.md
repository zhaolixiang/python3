# 一、日志记录的级别

1. debug：优先级10，记录调试的详细信息，只在调试时开启
2. info：优先级20，记录普通的消息，报告错误和警告等待。
3. warning：优先级30，记录相关的警告信息。
4. error：优先级40，记录错误信息、程序崩溃
5. critical：优先级50，记录错误信息

> 如果不设置，默认为iwarning

# 二、logging模块的主要结构

查看logging的源码，可知主要有四个类实现功能：

1. Loggers：提供应该程序直接使用的接口，如相关的配置设置
2. Handlers：将Loggers产生的日志传到指定位置，设置日志保存的位置；
3. Filters：对输出日志进行过滤操作；
4. Formatters：控制日志的输出格式

### Formatters

Formatters对象定义了日志的输出格式，有多种可选参数。

| 参数 | 含义 |
| :--- | :--- |
| %\(name\)s | Logger的名字 |
| %\(levellno\)s | 数字形式的日志级别 |
| %\(levelname\)s | 文本形式的日志级别 |
| %\(pathname\)s | 调用日志输出函数的模块的完整路径名，可能没有 |
| %\(filename\)s | 调用日志输出函数的模块的文件名 |
| %\(module\)s | 调用日志输出函数的模块名 |
| %\(funcName\)s | 调用日志输出函数的函数名 |
| %\(lineno\)d | 调用日志输出函数的语句所在的代码行 |
| %\(created\)f | 当前时间，用unix标表示的时间浮点表示 |
| %\(relativeCreated\)d | 输出日志信息时，自Logger创建以来的毫秒数 |
| %\(asctime\)s | 字符串形式的当前时间，默认格式是‘2018-11-22 16：49：45，896’，逗号后面是毫秒 |
| %\(thread\)d | 线程ID，可能没有 |
| %\(threadName\)s | 线程名，可能没有 |
| %\(process\)d | 进程ID，可能没有 |
| %\(message\)s | 用户输出的信息 |



