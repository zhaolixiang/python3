# 使用Redis存储配置信息

为了展示配置管理方面的难题是多么的常见，来看一个非常简单的配置例子：假设现在我们要用一个标志flag来表示web服务器是否正在进行维护，如果服务器正在进行维护，那么它就不应该发送数据库请求，而是应该向访问们返回一条简短的【抱歉，我们正在进行维护，请稍后重试】的信息；相反，如果服务器并没有进行维护，那么它就应该按照既定的程序来运行。

在通常情况下，即使只更新配置中的一个标志，也会导致更新后的配置文件被强制推送至所有Web服务器，收到更新的服务器可能需要重新载入配置、甚至可能还要重启应用程序服务器。

与其尝试为不断增多的服务写入和维护配置文件，不如让我们直接配置写入Redis里面。只要将配置信息存储在Redis里面，并编写应用程序来获取这些信息，我们就不用再编写工具来向服务器推送配置信息了，服务器和程序也不用再通过载入配置文件的方式 来更新配置信息了。

为了实现这个简单的功能，让我们假设自己已经构建了一个中间层或者插件，这个中间层额作用在于：当is\__under\_maintenance\(\)函数返回True时，它将向用户显示维护页面；与此相反，如何\_is_\_under\_maintenance\(\)函数返回False，它将如常地处理用户的访问请求。其中is\_under\_maintenance\(\)函数通过检查一个名为_is-under-maintenance的键来判断服务器是否正在进行维护：如果is-\_under-maintenance键非空，那么函数返回True；否则返回False，另外，因为访客在看见维护页面的时候通常都会不耐烦的频繁刷新页面，所以为了尽量降低Redis在处理高访问量Web服务器时的负载，is\_under\_maintenance\(\)函数最多只会每秒更新一次服务器维护信息。_

下面代码展示了_is\_under\_maintenance\(\)函数的具体定义：_

```
import time

LAST_CHECKED=None
IS_UNDER_MAINTENANCE=False

def is_under_maintenance(conn):
    #将连个变量设置为全局变量以便在之后对它们进行写入
    global LAST_CHECKED,IS_UNDER_MAINTENANCE
    #距离上次检查是否以及超过1秒？
    if LAST_CHECKED<time.time()-1:
        #更新最后检查时间
        LAST_CHECKED=time.time()
        #检查系统是否正在进行维护
        IS_UNDER_MAINTENANCE=bool(conn.get('is-under-maintenance'))
    #返回一个布尔值，用于表示系统是否正在进行维护。
    return IS_UNDER_MAINTENANCE
```

通过将is\__under_\_maintenance\(\)函数插入应用程序的正确位置上，我们可以在1秒内改变数以千计Web服务器的行为。为了降低Redis在处理高访问量web服务器时的负载，is\__under_\_maintenance\(\)函数将服务器维护状态信息的更新频率限制为最多每秒1次，但如果有需要的话，我们也可以加快信息的更新频率，甚至直接移除函数里面限制更新速度的那些代码。虽然is\__under_\_maintenance\(\)函数看上去似乎并不实用，但它的确展示了将配置信息存储在一个普通可访问位置的威力。

接下来我们要考虑的是，怎样才能将更复杂的配置选项存储到Redis里面呢？



