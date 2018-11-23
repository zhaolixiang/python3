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

实例：

```
import logging

#fmt:定义输出的日志信息的格式
#datefmt：定义时间信息的格式，默认为：%Y-%m-%d %H:%M:%S
#style:定义格式化输出的占位符，默认是%(name)格式，可选{}或$格式
formatter=logging.Formatter(fmt='%(asctime)s    %(levelname)s:  %(message)s'
                            ,datefmt='%Y-%m-%d %H:%M:%S',style='%')
```

### Handlers日志处理器

日志处理器用来处理日志的具体流向，是输出到文件中还是标准输出等，它通过设置Formatter控制输出格式，添加filters过滤日志。

##### 常用的处理器有两种

1. StreamHandler：用于向控制台打印日志
2. FileHandler：用于向日志文件打印日志

##### 其它的处理器

| 名称 | 详细位置 | 说明 |
| :--- | :--- | :--- |
| RotatingHandler | logging.handlers.RotatingHandler | 日志回滚方式，支持日志文件最大数量和日志文件回滚 |
| TimeRotatingHandler | logging.handlers.TimeRotatingHandler | 日志回滚方式，在一定时间区域内回滚日志文件 |
| SocketHandler | logging.handlers.SocketHandler | 远程输出日志到TCP/IP sockets |
| DatagramHandler | logging.handlers.DatagramHandler | 远程输出日志到UDP sockets |
| SMTPHandler | logging.handlers.SMTPHandler | 远程输出日志到邮件地址 |
| SysLogHandler | logging.handlers.SysLogHandler | 日志输出到syslog |
| NTEventLogHandler | logging.handlers.NTEventLogHandler | 远程输出日志到Windows NT/2000/xp的事件日志 |
| MemoryHandler | logging.handlers.MemoryHandler | 日志输出到内存中的指定buffer |
| HTTPHandler | logging.handlers.HTTPHandler | 通过“GET”或者“POST”远程输出到HTTP服务器 |

```
from logging import Handler

#所有日志处理器的父类
handler=Handler()

print('处理日志的等级：',handler.level)
print('处理日志的名字：',handler.name)
print('处理器的日志过滤器：：',handler.filters)
print('日志的格式：：',handler.filters)

#一些常用方法：
handler.get_name()
handler.set_name('')
handler.createLock()#创建线程锁
handler.acquire()#获取线程锁
handler.release()#释放线程锁
handler.setLevel('info') #设置日志处理器的记录级别
handler.setFormatter(fmt='')#设置日志的输出格式
handler.addFilter('')#往处理器中添加过滤器
handler.removeFilter('')#往处理器中移除过滤器
handler.emit('')#日志记录的处理逻辑，由子类实现
```

### Logger日志对象

Logger管理着所有记录日志的方法。

```
from logging import error, debug, warning, info, fatal, critical, getLogger

#返回一个Logger实例
#以'root'为名字的日志对象在Logger对象中只有一个实例
logger=getLogger('root')

print('获取根日志对象',logger.root)
print('获取manager',logger.manager)
print('获取根日志对象的名字',logger.name)
print('获取根日志对象记录水平',logger.level)
print('获取根日志对象过滤器列表',logger.filters)
print('获取根日志对象处理器列表',logger.handlers)
print('获取根日志对象',logger.disabled)

#设置日志记录水平
logger.setLevel('info')

#输出日志信息，格式化输出
logger.info('this is %s','info',exc_info=1)
#记录warning信息
logger.warning('')
#记录error信息
logger.error('')
#等价于logger.error('',exc_info=1)
logger.exception('')
#记录debug信息
logger.debug('')
#记录critical信息
logger.critical('')
#直接指定级别
logger.log('info','')

#添加处理器
logger.addHandler()
#移除处理器
logger.removeHandler()
#判是否有处理器
logger.hasHandlers()
```

# 三、logger的基本使用

实例：

```
import logging
import sys

def my_get_logger(appname):
    #获取logger实例，如果参数为空则返回root logger
    logger=logging.getLogger(appname)
    #创建日志输出格式
    formatter=logging.Formatter('%(asctime)s    %(levelname)s:  %(message)s')

    #指定输出的文件路径
    file_handler=logging.FileHandler('test.log')
    # 设置文件处理器，加载处理器格式
    file_handler.setFormatter(formatter)

    #控制台日志
    console_handler=logging.StreamHandler(sys.stdout)
    console_handler.formatter=formatter

    #为logger添加的日志处理器
    logger.addHandler(file_handler)
    logger.addHandler(console_handler)

    #指定日志的最低输出级别，默认为warn级别
    logger.setLevel(logging.INFO)
    return logger

if __name__ == '__main__':
    logger=my_get_logger('test')
    logger.debug('this is debug info')
    logger.info('this is information')
    logger.warning('this is warning message')
    logger.error('this is error message')
    logger.fatal('this is fatal message,it is same ad logger.critical')
    logger.critical('this is critical message')
```

结果：

```
2018-11-22 16:31:34,023    INFO:  this is information
2018-11-22 16:31:34,023    WARNING:  this is warning message
2018-11-22 16:31:34,023    ERROR:  this is error message
2018-11-22 16:31:34,024    CRITICAL:  this is fatal message,it is same ad logger.critical
2018-11-22 16:31:34,024    CRITICAL:  this is critical message
```

# 四、logger日志记录的逻辑调用过程

1. 记录日志通过调用logger.debug等方法；
2. 首先判断本条记录的日志级别是否大于设置的级别，如果不是，直接pass，不再执行；
3. 将日志信息当做参数创建一个LogRecord日志记录对象
4. 将LogRecord对象经过logger过滤器过滤，如果被过滤则pass
5. 日志记录对象被Handler处理器的过滤器过滤
6. 判断本条记录的日志级别是否大于Handler处理器设置的级别，如果不是，直接pass，不再执行；
7. 最后调用处理器的emit方法处理日志记录；

# 五、配置logger

1. 通过代码进行完整配置，主要是通过getLogger方法实现，但不好修改
2. 通过basicConfig方法实现，这种方式快速但不够层次分明
3. 通过logging.config.fileConfig\(filepath\)，文件配置
4. 通过dictConfig的字典方式配置，这是py3.2版本引入的新的配置方法

### 使用文件方式配置

```
#logging.cong

[loggers]  
#定义日志的对象名称是什么，注意必须定义root，否则报错
keys=root,main

[handlers]
#定义处理器的名字是什么，可以有多个，用逗号隔开
#kes=consoleHandler

[formatters]
#定义输出格式对象的名字，可以有多个，用逗号隔开
keys=simpleFormatter

[logger_root]
#配置root对象的日志记录级别和使用的处理器
level=INFO
handlers=consoleHandler

[logger_main]
#配置main对象的日志记录级别和使用的处理器，qualname值得就是日志对象的名字
level=INFO
handlers=consoleHandler
quanlname=main
#logger对象把日志传递给所有相关的handler的时候，会逐级向上寻找这个logger和它所有的父logger的全部handler，
#propagate=1表示会继续向上搜寻；
#propagate=0表示停止搜寻，这个参数涉及重复打印的坑。
propagate=0
```



