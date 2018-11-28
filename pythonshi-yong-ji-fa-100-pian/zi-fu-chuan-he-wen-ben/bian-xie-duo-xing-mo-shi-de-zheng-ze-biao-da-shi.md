# 1、需求🙀

> 我们打算用正则表达式对一段文本块做匹配，但是希望在进行匹配时能够跨越多行。

# 2、解决方案😸

这个问题一般出现在希望使用句点\(.\)来匹配任意字符，但是忘记了句点并不能匹配换行符。

实例：假设向匹配C语言风格的注释：

```
import re

str_pat=re.compile(r'/\*(.*?)\*/')
text1='/* mark say "love" */'
text2='''/* mark say "love",'
      'jingjing say "yes" */'''
print(str_pat.findall(text1))
print(str_pat.findall(text2))
```

结果：

```
[' mark say "love" ']
[]
```

要解决这个问题，可以添加对换行符的支持。

实例：

```

```

结果：

```

```

1

111

1

1

# 3、分析😈



