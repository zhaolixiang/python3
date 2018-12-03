# 自动Redis连接管理

手动创建和传递Redis连接并不是一件容易地事情，这不仅是因为我们需要重复查阅配置信息，还有一个原因就是，即使使用了 上一节介绍的配置管理函数，我们还是需要获取配置、连接Redis，并在使用完连接之后关闭连接。为了简化连接的管理操作，我们将编写一个装饰器，让它负责连接除配置服务器之外的所有其他Redis服务器。

> 装饰器
>
> Python提供了一种语法，用于将函数X传入另一个函数Y的内部，其中函数Y就被成为装饰器。装饰器给用户提供了一个修改函数X行为的机会。有些装饰器可以用于校验参数，而有些装饰器则可以用于注册回调函数，甚至还有一些装饰器可以用于管理连接：就像我们接下来要做的那样。

下面代码展示了我们定义的装饰器，它接受一个指定的配置作为参数并生成一个包装器，这个包装器可以包裹一个函数，使得之后对被包裹函数的调用可以自动连接至正确的Redis服务器，并且连接Redis服务器所使用的那个连接会和用户之后提供的其他参数一同传递至包裹的函数：

```
REDIS_CONNECTIONS={}

#将应用组件的名字传递给装饰器
def redis_connection(component,wait=1):
    #因为函数每次被调用都需要获取这个配置键，所以我们干脆把它缓存起来
    key='config:redis:'+component
    #包装器接受一个函数作为参数，并使用另一个函数来包裹这个函数。
    def wrapper(function):
        #将被包裹函数的一些有用的元数据复制给配置处理器。
        @function.wraps(function)
        def call(*args,**kwargs):#创建负责管理连接信息的函数
            #如果有就配置存在，那么获取它
            old_config=CONFIGS.get(key,object())
            #如果有新配置存在，那么获取它
            _config=get_config(config_connection,'redis',component,wait)

            config={}
            #对配置进行处理并将其用于创建Redis连接
            for k,v in _config.iteritems():
                config[k.encode('utf-8')]=v

            #如果新旧配置并不相同，那么创建新的连接
            if config!=old_config:
                REDIS_CONNECTIONS[key]=redis.Redis(**config)

            #将Redis连接以及其他匹配的参数传递给包裹函数，然后调用该函数并返回它的执行结果。
            return function(REDIS_CONNECTIONS.get(key),*args,**kwargs)
        #返回被包裹的函数
        return call
    #返回用于包裹Redis函数的包装器
    return wrapper
```

> 同时使用\*args和\*\*kwargs
>
> 在Python中，函数定义的args变量用于获取所有位置参数，而kwargs变量则用于获取所有命令出纳和素，这两种参数传递方式都可以将给定的参数传入被调用的函数里面。

上面战术的一系列嵌套函数初看上去可能会让人感动头昏目眩，但它们实际上并没有想象中的那么复杂。redis\__connection\(\)装饰器接受一个应用组件的名字作为参数并返回一个包装器。这个包装器接受一个我们想要将连接传递给它的函数为参数，然后对函数进行包裹并返回被包裹函数的调研器。这个调用器负责处理所有获取配置信息的工作，除此之外，它还负责连接Redis服务器并调用被包裹的函数。尽管redis_connecition\(\)函数描述起来相当复杂，但实际使用起来却是非常方便的，下面代码就展示了怎样将redis_\_connection\(\)函数应用到之间介绍的log\_recent\(\)函数上面。_

