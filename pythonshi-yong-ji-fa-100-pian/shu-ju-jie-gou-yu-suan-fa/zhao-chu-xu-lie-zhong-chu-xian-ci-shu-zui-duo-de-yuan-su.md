# 1、需求🙀

> 我们有一个元素序列，想知道在序列中出现次数最多的元素是什么？

# 2、解决方案😸

collections模块中国的Counter类正是为此类问题而设计的。它甚至有一个非常方便的most\_common\(\)方法可以告诉我们答案。可以给Counter对象提供任何可哈希的对象序列作为输入。

* ##### 实例：假设一个列表，其中有一些列的单词，我们想找出哪些单词出现的最为频繁：

```
from collections import Counter
words=[
    'a','b','c','d','e','f',
    'a','b','c','d','e','f',
    'a','b','c',
    'a','b',
    'a'
]
#利用Counter统计每个元素出现的个数
words_counts=Counter(words)
#出现次数最多的3个元素
top_three=words_counts.most_common(3)
#返回元素和出现次数
print(top_three)
```

运行结果：

```
[('a', 5), ('b', 4), ('c', 3)]
```



