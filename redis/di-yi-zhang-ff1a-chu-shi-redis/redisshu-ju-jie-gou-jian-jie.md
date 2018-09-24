Redis可以存储键与5种不同的数据结构类型之间的映射，这5中数据结构类别分别是：STRING（字符串）、LIST（列表）、SET（集合）、HASH（散列）和ZZET（有序集合）。

大部分程序员应该都不会对STRING、LIST、HASH这三种结构感到陌生，因为他们和很多编程语言内建的字符串、列表和散列等结构在实现和语义（semantics）方面都非常类似。有些编程语言还有集合数据结构，在实现和语义上类似于Redis的SET。ZSET在某种程度上是一种Redis特有的结构，但是当你熟悉它之后，就会发现它也是一种非常有用的结构。下表对比了Redis提供的5种结构，说明了这些结构存储的值，并简单介绍了他们的语音。

| 结构类型 | 结构存储的值 | 结构的读写能力 |
| :--- | :--- | :--- |
| STRING | 可以是字符串、整数、浮点数 | 对整个字符串或者字符串的其中一部分执行操作，对整数或浮点数执行自增（increment）或者自减（decrement）操作。 |
| LIST | 一个链表，链表上的每个节点都包含了一个字符串 | 从链表的两端推入或者弹出元素；根据偏移量对链表进行修剪（trim）；读取单个或者多个元素；根据值查找或则移除元素。 |
| SET | 包含字符串的无序收集器（unordered collection），并且被包含的每个字符串都是独一无二、各不相同 | 添加、获取、移除单个元素；检查一个元素是否存在于集合中；计算交集、并集、差集；从集合中随机获取元素 |
| HASH | 包含键值对的无序散列表 | 添加、获取、移除单个键值对；获取所有键值对。 |
| ZSET（有序集合） | 字符串成员（member）与浮点数分值（score）之间的有序映射，元素的排列顺序由分值的大小决定 | 添加、获取、删除单个元素；根据分值范围（range）或者成员来获取元素。 |


