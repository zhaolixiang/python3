### 2、发布并获取文章

* 发布一篇文章首先需要创建一个新的文章ID，这项工作可以通过对一个计数器（counter）执行incr命令来完成。
* 接着程序需要使用sadd将文章发布者的ID添加到记录文章已投票用户名单的集合里面，并使用expire命令为这个集合设置一个过期时间，让Redis在文章发布期满一周之后自动删除这个集合。
* 之后，程序会使用hmset命令来存储文章的相关信息，并执行两个zadd命令，将文章的初始评分（initial score）和发布时间分别添加到两个相应的有序集合里面。

```
#发布文章
def post_article(conn,user,title,link):
    article_id=str(conn.incr('article:'))

    voted='voted:'+article_id

    conn.sadd(voted,user)
    conn.expire(voted,ONE_WEEK_IN_SECONDS)

    now=time.time()
    article='article:'+article_id
    conn.hmset(article,{
        'title':title,#文章标题
        'link':link,#文章链接
        'poster':user,
        'time':now,
        'votes':1
    })

    conn.zadd('score:',article,now+VOTE_SCORE)
    conn.zadd('time',article,now)

    return article_id
```

好了，我们已经陆续实现了文章的投票功能和文章发布功能，接下来要考虑的就是如何取出评分最高的文章已经如何取出最新发布的文章。

为了实现这两个功能，

* 首先需要使用zrevrange命令取出多个文章ID
* 然后再对每个文章ID执行一次hgetall命令来取出文章的详细信息。



