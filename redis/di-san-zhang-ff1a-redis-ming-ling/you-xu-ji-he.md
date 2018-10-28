和散列存储着键与值之间的映射类似，有序集合也存储着成员与分值之间的映射，并且提供了分值处理命令，已经根据分值大小有序的获取（fetch）和扫描（scan）成员和分值的命令。本书曾在第一章使用有序集合实现过基于发布时间排序的文章列表和基于投票数量排序的文章列表，还在第二章使用有序集合存储过cookie的过期时间。

> 这些分值在Redis中以IEEE754双精度点数的格式存储。

本节将对操作有序集合的命令进行介绍，其中包括向有序集合添加新元素的命令、更细已有元素的命令、以及对有序集合进行交集运算和并集运算的命令。

下表展示了一部分常用的有序集合命令：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| zadd | zadd key-name score memeber \[score member ...\] | 将带有给定分值的成员添加到有序集合里面 |
| zrem | zrem key-name member \[member ...\] | 从有序集合里面移除给定的成员，并返回被移除成员的数量 |
| zcard | zcard key-name | 返回有序集合包含的成员数量 |
| zincrby | zincrby key-name increment member | 将merber成员的分值加上increment |
| zcount | zcount key-name min max | 返回分值介于min和max之间的成员数量 |
| zrank | zrank key-name member | 返回成员member有序集合中的排名。 |
| zscore | zsore key-name member | 返回成员member的分值 |
| zrange | zrange key-name start stop \[WITHSCORES\] | 返回有序集合中排名介于start和stop之间的成员，如果给定了可选的WITHSCORES选项，那么命令会将成员的分值也一并返回。 |



