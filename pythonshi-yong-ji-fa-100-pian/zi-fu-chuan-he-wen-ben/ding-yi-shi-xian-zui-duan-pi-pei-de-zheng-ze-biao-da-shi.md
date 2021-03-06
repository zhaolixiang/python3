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

结果：

```
['love']
['love",jingjing say "yes']
```

在这个例子中，模式r'\"\(.\*\)\"'尝试去匹配包含在引号中的文本。但是，\*操作符在正则表达式中采用的是贪心策略，所以匹配过程是基于找出最长的可能匹配来进行的。所以上面才会出现【love",jingjing say "yes】这个匹配结果。

要解决这个问题，只要在模式中的\*操作符后面加上?修饰符就可以了。

示例：

```
import re

str_pat=re.compile(r'\"(.*?)\"')
text1='mark say "love"'
text2='mark say "love",jingjing say "yes"'
print(str_pat.findall(text1))
print(str_pat.findall(text2))
```

结果：

```
['love']
['love', 'yes']
```

这么做使得匹配过程不会以贪心方式进行，也就会产生最短的匹配了。

# 3、分析😈

本节提到了一个当编写还有句点【.】字符的正则表达式常会遇到的问题。为了解决最长匹配问题，让匹配变为最短匹配，需要在\*或+后加上一个【?】。

