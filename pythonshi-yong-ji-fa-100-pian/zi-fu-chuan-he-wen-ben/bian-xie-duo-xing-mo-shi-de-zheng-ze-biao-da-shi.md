# 1、需求🙀

> 我们打算用正则表达式对一段文本块做匹配，但是希望在进行匹配时能够跨越多行。

# 2、解决方案😸

这个问题一般出现在希望使用句点\(.\)来匹配任意字符，但是忘记了句点并不能匹配换行符。

实例：假设向匹配C语言风格的注释：

```
import re

str_pat=re.compile(r'/\*(.*?)\*/')
text1="/* mark */"
text2='''/* mark 
            2018    */'''
print(str_pat.findall(text1))
print(str_pat.findall(text2))
```

结果：

```
[' mark ']
[]
```

要解决这个问题，可以添加对换行符的支持。

实例：

```
import re

#将.换成(?:.|\n)
str_pat=re.compile(r'/\*((?:.|\n)*?)\*/')
text1="/* mark */"
text2='''/* mark 
            2018    */'''
print(str_pat.findall(text1))
print(str_pat.findall(text2))
```

结果：

```
[' mark ']
[' mark \n            2018    ']
```

\(?:.\|\n\)指定了一个非捕获组（即，这个组只做匹配但不捕获结果，也不会分配组号）。

# 3、分析😈



