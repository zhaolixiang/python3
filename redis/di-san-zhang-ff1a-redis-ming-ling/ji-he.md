Redis的集合以无序的方式来存储多个各不相同的元素，用户可以快速地对集合执行添加元素操作、移除元素操作、以及检查一个元素是否存在于集合里。本节将对最常用的集合命令进行介绍，包括：插入命令、移除命令、将元素从一个集合移动到另一个集合的命令、以及对多个集合执行交集运算、并集运算、差集运算的命令。

下表展示了其中一部分最常用的集合命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| sadd | sadd key-name item \[item ...\] |  |
| srem | srem key-name item \[item ...\] |  |
| sismember | sismember key-name item |  |
| scard | scard key-name |  |
| smembers | smembers key-name |  |
| srandmember | srandmember key-name \[count\] |  |
| spop | spop key-name |  |
| smove | smove source-key dest-key item |  |



