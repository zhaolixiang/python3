静态文件下载是大多数网站必备的功能，与静态文件相关的开发工作有两类：配置静态文件路径和优化静态文件访问。

# 1、配置静态文件路径

配置静态文件路径的目的在于为客户端提供静态文件的可访问性。Tornado提供了两种方式进行配置静态文件URL路径与服务器本地路径的关联关系。

### （1）static目录配置

在tornado.web.Application的构造函数中可以传入static\_path参数，用于配置对URL路径[http://mysite.com/static中文件的本地路径，比如：](http://mysite.com/static中文件的本地路径，比如：)

```
import tornado


def make_app():
    return tornado.web.Application([
        #此处写入映射
    ],
        static_path="C:\\www\\static"
    )
```

这将使诸如[http://mysite.com/static/favorite.png、http://mysite.com/static/css/main.cs这的文件的访问映射到C:\www\static中。](http://mysite.com/static/favorite.png、http://mysite.com/static/css/main.cs这的文件的访问映射到C:\www\static中。)

通常这些静态文件的目录与网站的代码文件有某种相对关联关系，可以通过下面这样的方法将该参数设置为相对路径：

```
import os
import tornado


def make_app():
    return tornado.web.Application([
        #此处写入映射
    ],
        static_path=os.path.join(os.path.dirname(__file__),'static')
    )
```

即指定静态目录为本程序文件所在目录的static子目录。

