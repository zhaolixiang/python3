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

这个方法既可以用于取出评分最高的文章，又可以用于取出最新发布的文章。

> 注意：因为有序集合根据成员的分值从小到大排列，所以使用zrevrange命令，以【分值从大到小】的排列顺序取出文章ID才是正确的做法。

```
ARTICLES_PER_PAGE=25 #每页展示数量

def get_articls(conn,page,order='score:'):
    start=(page-1)*ARTICLES_PER_PAGE  #设置文章的起始索引
    end=start+ARTICLES_PER_PAGE-1     #设置文章的结束索引

    ids=conn.zrevrange(order,start,end) #获取多个文章ID

    articles=[]

    for id in ids:
        article_data=conn.hgetall(id)
        article_data['id']=id
        articles.append(article_data)
    return articles
```

虽然我们构建的网站现在已经可以展示最新发布的文章和评分最高的文章了，但它还不具备目前很多投票网站都支持的群组【group】功能：可以让用户看见与特定话题有关的文章，比如：【可爱的动物】、【历史】、【政治】等等。

