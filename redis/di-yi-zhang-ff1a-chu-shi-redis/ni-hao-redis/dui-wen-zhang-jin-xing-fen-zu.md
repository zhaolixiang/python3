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



