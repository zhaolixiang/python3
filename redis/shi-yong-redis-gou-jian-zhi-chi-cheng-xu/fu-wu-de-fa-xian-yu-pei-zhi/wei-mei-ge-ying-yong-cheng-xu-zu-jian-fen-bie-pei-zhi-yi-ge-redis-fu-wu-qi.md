# 为每个应用程序组件分别配置一个Redis服务器

在我们越来越多地使用Redis的过程中，无数的开发者已经发现，最终在某个时间点上，只使用一台Redis服务器将不能满足我们的需要。因为我们可能需要记录更多信息，可能需要更多用于缓存的空间，还可能会使用本书之后的章节会介绍的、使用Redis构建的高级服务。但不管何种原因，我们都需要用到更多Redis服务器。

为了平滑地从单台服务器过渡到多台服务器，用户最好还是为应用程序中的每个独立部分都分别运行一个Redis服务器，比如说，一个专门负责记录日志、一个专门负责记录统计数据、一个专门负责进行缓存、一个专门负责存储cookies等。别忘了，一台机器是可以运行多个Redis服务器的，只要这些服务器使用的端口号各不同就可以了。除此之外，在一个Redis服务器里面使用多个【数据库】，也可以减少系统管理的工作量。以上提到的两种方法，都是通过将不同数据划分至不同键空间的方式，来或多或少的简化迁移至更大或更多服务器时所需的工作。但遗憾的是，随着Redis服务器的数量或者Redis数据库的数量不断增多，为所有Redis服务器管理和分发配置信息的工作将变得越来越烦琐和无趣。

在上一节中，我们用了Redis来存储表示服务器是否正在进行维护的标志，并通过这个标志来决定是否需要向访客显示维护页面。而这一次，我们同样可以使用Redis来存储与其他Redis服务器有关的信息。说的更详细一点，我们可以把一个已知的Redis服务器用作配置信息字典，然后通过这个字典存储的配置信息来连接为不同应用或服务组件提供数据的其他Redis服务器。此外，这个字典还会在配置出现变更时，帮助客户端连接至正确的服务器。字典的具体实现比这个例子所要求的更为通用一些，因为我敢肯定，当你开始使用这个字典来获取配置信息的时候，你很快就会把它应用到其他服务器以及其他服务上面，而不仅仅用于获取Redis服务器的配置信息。

我们将构建一个函数，该函数可以从一个键里面取出一个JSON编码的配置值，其中，存储配置值的键由服务的类型以及使用该服务的应用程序命名。举个例子，如何我们想要获取连接存储统计数据的Redis服务器所需的信息，那么就需要获取config:redis:statistics键的值。下面函数展示了设置配置值的具体方法：

```
def set_config(conn,type,component,config):
    conn.set('config:%s:%s'%(type,component))
    json.dumps(config)
```

通过这个函数，我们可以随心所欲的设置任何JSON编码的配置信息。因为get\__config\(\)函数和前面介绍过的is_\_under_\_maintenance\(\)函数具有相似的结构，所以我们只要在语义上稍作修改，就可以使用get\_config\(\)函数来替代is_\_under\_maintenance\(\)函数。下面代码列出了与set\_config\(\)相对应的get\_config\(\)函数，这个函数可以按照用户的需要，对配置信息进行0秒、1秒或者10秒的局部缓存。

```
import json
import time

CONFIGS={}
CHECKED={}

def get_config(conn,type,component,wait=1):
    key='config:%s:%s'%(type,component)
    #检查是否需要对这个组件的信息进行更新
    if CHECKED.get(key)<time.time()-wait:
        #有需要对配置进行更新，记录最后一次检查这个连接的时间
        CHECKED[key]=time.time()
        #取得Redis存储的组件配置
        config=json.loads(conn.get(key) or '{}')
        #将潜在的Unicode关键字参数转换为字符串的关键字参数
        config=dict((str(k),config[k]) for k in config)
        #取得组件正在使用的配置
        old_cofig=CONFIGS.get(key)
        #如果两个配置并不相同
        if config!=old_cofig:
            #那么对组件的配置进行更新
            CONFIGS[key]=config

    return CONFIGS.get(key)
```

在拥有了配置信息和获取配置信息的两个函数之后，我们还可以在此之上更近一步。我们在前面一直考虑的都是怎样存储和获取配置信息以便连接各个不同的Redis服务器，但直到目前为止，我们编写的绝大多数函数和第一个参数都是一个连接参数。因此，为了不再需要手动获取我们正在使用的各项服务的连接，下面让我们来构建一个能够帮助我们自动连接这些服务的方法。

