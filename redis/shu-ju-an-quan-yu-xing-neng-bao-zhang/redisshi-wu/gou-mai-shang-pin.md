# 购买商品

下面的函数展示了从市场里面购买一件商品的具体方法：程序首先使用watch对市场以及买家的个人信息进行监视，然后获取买家拥有的钱数以及商品的售价，并检查买家是否有足够的钱来购买该商品。如果买家没有足够的钱，那么程序会取消事务；相反，如果买家的钱足够，那么程序首先会将买家支付的钱转移给卖家，然后将售出的商品移动至买家的包裹，并将该商品从市场中移除。当买家的个人信息或者商品买卖市场出现变化而导致WatchError移除出现时，程序进行重试，其中最大重试时间为10秒：

```
import time
import redis

def purchase_item(conn,buyerid,itemid,sellerid,lprice):
    buyer='users:%s'%buyerid
    seller='users:%s'%sellerid
    item="%s.%s"%(itemid,sellerid)

    inventory="inventory:%s"%buyerid
    end=time.time()+10
    pipe=conn.pipeline()

    while time.time()<end:
        try:
            #对商品买卖市场以及买家对个人信息进行监视
            pipe.watch("market:",buyer)

            #检查买家想要购买的商品的价格是否出现了变化
            #以及买家是否有足够的钱来购买这件商品
            price=pipe.zscore("market:",item)
            funds=int(pipe.hget(buyer,'funds'))
            if price!=lprice or price>funds:
                pipe.unwatch()
                return None

            #先将买家支付的钱转移给卖家，然后再将购买的商品移交给买家
            pipe.multi()
            pipe.hincrby(seller,"funds",int(price))
            pipe.hincrby(buyer,'funds',int(-price))
            pipe.sadd(inventory,itemid)
            pipe.zrem("market:",item)
            pipe.execute()
            return True
        except redis.exceptions.WatchError:
            #如果买家的个人信息或者商品买卖市场在交易的过程中出现了变化，那么进行重试。
            pass
    return False
```

在执行商品购买操作定位时候，程序除了需要花费大量时间来准备相关数据之外，还需要对商品买卖市场以及买家的个人信息进行监视：监视商品买卖市场是为了确保买家想要购买的商品仍然有售（或者在商品已经被其他人买走时进行提示），而监视买家的个人信息则是为了验证买家是否有足够的钱来购买自己想要的商品。

当程序确认商品仍然存在并且买家有足够的钱的时候，程序会将被购买的商品移动到买家的包裹里面，并将买家支付的钱转移给卖家。

在观察了市场上展示的商品之后，Bill（用户ID为27）决定购买Frank在市场上销售的ItemM，下图展示了购买操作执行期间，数据结构的变化：



