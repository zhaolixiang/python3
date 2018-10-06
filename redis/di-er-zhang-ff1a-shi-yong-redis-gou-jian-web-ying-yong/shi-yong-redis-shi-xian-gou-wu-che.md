网景（Netscape）公司在20世纪90年代中期最先在网络中使用了cookie，这些cookie最终变成了我们现在使用的cookie。cookie最初的意图是在于为网络销售商提供一种购物车，让用户可以收集他们想要购买的商品。

使用cookie实现购物车：也就是将整个购物车都存储到cookie里面的做法非常常见，这种做法的一大优点是无须对数据库进行写入就可以实现购物车功能，而缺点则是程序需要重新解析和验证（validata）cookie，确保cookie的格式正确，并且包含的商品都是真正可购买的商品。cookie购物车还有一个缺点：因为浏览器每次发送请求都会连cookie一起发送，所以如果购物车cookie的体积比较大，那么请求发送和处理的速度可能会有所降低。

因为我们在签名已经使用了Redis实现了会话cookie和记录用户最近浏览过的商品这两个特性，所以我们决定将购物车的信息也存储到Redis里面，并且使用与会话cookie相同的cookieID来引用购物车。

购物车的定义非常简单：每个用户的购物车都是一个散列，这个散列存储了商品ID与商品订购数量之间的映射。对商品数量进行验证的工作有web应用程序负责，我们要做的则是在商品的订购数量出现变化时，对购物车进行更新：如果用户订购某件商品的数量大于0，那么程序会将这件商品的ID以及用户订购该商品的数量添加到散列里面，如果用户购买的商品以及存在于散列里面，那么新的订购数量会覆盖已有的订购数量；相反的，如果用户订购某件商品的数量不大于0，那么程序将从散列里面移除该条目。

```
def add_to_cart(conn,session,item,count):
    if count<=0:
        #从购物车里面移除指定商品
        conn.hrem('cart:'+session,item)
    else:
        #将指定的商品添加到购物车🛒
        conn.hset('cart:'+session,item,count)
```

接着，我们需要对之前的会话清理函数进行更新，让它在清理会话的同时，将旧会话对应用户的购物车也一并删除：

```
#清理旧会话
import time

QUIT=False
LIMIT=10,000,000

def clean_sessions(conn):
    while not QUIT:
        #目前已有令牌的数量
        size=conn.zcard('recent:')
        if size<=LIMIT:
            #令牌数量未超过限制，休眠1秒后再重新检查
            time.sleep(1)
            continue
        end_index=min(size-LIMIT,100)
        tokens=conn.zrange('recent:',0,end_index-1)

        session_keys=[]
        #为那些将要删除的令牌构建键名
        for token in tokens:
            session_keys.append('viewed:'+token)
            #新增下面有一行代码用于删除旧会话对应用户的购物车
            session_keys.append('cart:'+token)
        #移除最旧的那些令牌
        conn.delete(*session_keys)
        conn.hdel('login:',*tokens)
        conn.zrem('recent:',*tokens)
```

我们现在讲会话和购物车都存储到了Redis里面，这种做法除了可以减少请求的体积之外，还使得我们可以根据用户浏览过的商品、用户放入购物车的商品以及用户最终购买的商品进行统计计算，并构建起很多大型网络零售商都在提供的【在查看过这件商品的用户当中，有X%的用户最终购买了 这件商品】、【购买了这件商品的用户也购买了XXX商品】等功能，这些功能可以帮众用户查找其它相关的商品，并最终提升网站的销售业绩。

通过将会话cookie和购物车cookie存储在Redis里面，我们得到了进行数据分析所需的两个重要的数据来源，接下来将展示如果使用缓存来减少数据库和Web前端的负载。

