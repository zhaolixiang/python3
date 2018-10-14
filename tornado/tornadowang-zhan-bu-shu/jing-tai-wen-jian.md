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

### （2）StaticFileHandler配置

如果除了[http://mysite.com/static目录还有其他存放静态文件的URL，则可以用RequestHandler的子类StaticFileHandler进行配置，比如：](http://mysite.com/static目录还有其他存放静态文件的URL，则可以用RequestHandler的子类StaticFileHandler进行配置，比如：)

```
def make_app():
    return tornado.web.Application([
        #此处写入映射

        #这里配置了3个StaticFileHandler
        (r'/css/(.*)',tornado.web.StaticFileHandler,{'path':'assets/css'}),
        (r'/images/png/(.*)',tornado.web.StaticFileHandler,{'path':'assets/image'}),
        (r'/js/(.*)',tornado.web.StaticFileHandler,{'path':'assets/js','default_filename':'templates/index.html'}),
    ],
        static_path=os.path.join(os.path.dirname(__file__),'static')
    )
```

本例中除了static\_path，还用StaticFileHandler配置了另外3个静态文件目录。

* 所有对http://mysite.com/css/\*的访问被映射到相对路径assets/css中。
* 对http://mysite.com/images/png/\*的访问被映射到assets/images目录中。
* 对http://mysite.com/js/\*的访问被映射到assets/js目录中；该条StaticFileHandler的参数中还被配置了default\_filename参数，即当用户访问了http://mysite.com/js目录本身时，将返回templates/index.html文件。

# 2、优化静态文件访问

优化静态文件访问的目的在于减少静态文件的重复传送，提高网络及服务器的利用效率，通过在模板文件中用static\_url方法修饰静态文件链接可以达到这个目的：

```

```



