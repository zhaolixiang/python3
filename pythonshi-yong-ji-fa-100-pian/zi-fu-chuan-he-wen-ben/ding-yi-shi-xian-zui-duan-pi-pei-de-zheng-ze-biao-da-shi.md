# 1、需求🙀

> 我们正在尝试用正则表达式对文本模式做匹配，但识别出来的是最长的可能匹配。相反，我们想将其修改为最短的可能匹配。

# 2、解决方案😸

这个问题通常会在匹配的文本被一对开始和结束的分隔符包起来的时候出现（例如带引号的字符串），为了说明这个问题，请看下面实例：

```
import re

str_pat=re.compile(r'\"(.*)\"')
text1='mark say "love"'
text2='mark say "love",jingjing say "yes"'
print(str_pat.findall(text1))
print(str_pat.findall(text2))

```

1

1

1

1

1

1

1

# 3、分析😈



