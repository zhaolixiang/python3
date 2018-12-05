# 即时查询

说一个系统支持即时查询的意思就是无需预先定义系统接收的查询类型。关系型数据库有这个能力，它们会严格遵照指示执行任何完备的SQL查询，无论有什么条件。如果你进使用过关系型数据库，那么会认为即时查询是理所应当的。但是，并非所有的数据库都支持动态查询。举例来说，键值存储只能按一个维度来查询：键。和很多其他系统一样，键值存储牺牲了丰富的查询能力来换取一个简单的可伸缩模型。关系型数据库世界中，查询能力是最基础不过的事情，MongoDB的设计目标之一就是尽可能保留这种能力。

要了解MongoDB的查询语句如何工作，让我们先来看一个简单的例子，它设计文档和评论。假设想要找到所有带有politics标签、投票数大于10的文章，SQL查询大概会是这样的：

```
SELECT * FROM posts
    INNER JOIN posts_tags ON posts.id=posts_tags.post_id
    INNER JOIN tags ON posts_tags.tag_id==tags.id
    WHERE tags.text='politics' AND posts.vote_count>10;
```

MongoDB中的等效查询是用文档来做匹配的，特殊的【$gt】表示【大于】：

```
dp.posts.find({'tags':'politics','vote_count':{'$gt':10}});
```

请注意，这两个查询采用了不同的数据模型。SQL查询依赖于严格正规化的模型，其中文章和标签保存在不同的数据表中，而MongoDB的查询假定标签是存储在每个文章的文档中。两者都演示了对任意属性组合执行查询的能力，这是即时查询的本质。

