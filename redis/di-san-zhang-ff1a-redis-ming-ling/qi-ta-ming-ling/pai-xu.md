Redis的排序操作和其他编程语言的排序操作一样，都可以根据某种比较规则对一系列元素进行有序排列。负责执行排序操作的sort命令可以根据字符串、列表、集合、有序集合、散列这5种键Kim存储的数据，对列表、集合以及有序集合进行排序。如果读者之前曾经使用过关系数据库的话，那么可以将soft命令看做是sql语言里的order by。

下表展示了sort命令的定义：

| 命令 | 用例 | 用例描述 |
| :--- | :--- | :--- |
| soft | soft source-key  \[by pattern\]  \[limit offset count\] \[get pattern \[get pattern ...\]\] \[asc\|desc\] \[alpha\] \[store dest-key\] | 根据给定的选项，对输入列表、集合或者有序集合进行排序，然后返回或者存储排序的结果。 |



