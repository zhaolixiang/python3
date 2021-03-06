### 3、对文章进行分组

群组功能由两个部分组成，一个部分负责记录文章属于哪个群组，另一部分负责取出群组里面的文章。为了记录各个群组都保存了哪些文章，网站需要为每个群组创建一个集合，并将所有同属一个群组的文章ID都记录到那个集合里面。

```
#将文章添加到群组里面的方法，以及从群组里面移除文章的方法
def add_remove_groups(conn,article_id,to_add=[],to_remove=[]):
    article='article:'+article_id
    for group in to_add:
        conn.sadd('group:'+group,article) #将文章添加到它所属的群组里面
    for group in to_remove:
        conn.srem('group:'+group,article) #从群组里面移除文章
```

初看上去，可能会有读者觉得使用集合来记录群组文章并没有多大用处。到目前为止，我们只看到了集合结构检查某个元素是否存在的能力，但实际上Redis不仅可以对多个集合执行操作，甚至在一些情况下，还可以在集合和有序集合之间执行操作。

为了能够根据评分对群组文章进行排序和分页，网站需要将同一个群组里面的所有文章都按照评分有序的存储到一个有序集合里面。Redis的zinterstore命令可以接受多个集合和多个有序集合作为输入，找出所有同时存在于集合和有序集合的成员，并以几种不同的方式来合并【combine】这些成员的分值（所有集合成员的分值都会被视为：1）。对于我们的文章投票网站来说，程序需要使用zinterstore命令选出相同成员中最大的那个分值来作为交集成员的分值：取决于所使用的排序选项，这些分值既可以是文章的评分，也可以是文章的发布时间。

通过对存储群组文章的集合和存储文章评分的有序集合执行zinterstore命令，程序可以得到按照文章评分顺序的群组文章；

而通过对存储群组文章的集合和存储文章发布时间的有序集合执行zinterstore命令，程序则可以得到按照文章发布时间排序的群组文章。如果群组包含的文章非常多，那么执行zinterstore命令就会比较花时间，为了尽量减少Redis的工作量，程序会将这个命令的计算结果缓存60秒。

```
#分页并获取群组文章
def get_group_articles(conn,group,page,order='score:'):
    key=order+group #为每个群组的每种排列顺序都创建一个键
    #检查是否已有缓存的排序结果，如果没有的话现在就进行排序
    if not conn.exists(key):
        conn.zinterstore(key,
                         ['group:'+group,order],
                         aggregate='max')
        conn.expire(key,60) #让redis在60秒后自动删除这个有序集合
    return get_articls(conn,page,key)
```

> 友情提示，上面只是部分代码，无法直接运行，只需要先理解思路就行。



