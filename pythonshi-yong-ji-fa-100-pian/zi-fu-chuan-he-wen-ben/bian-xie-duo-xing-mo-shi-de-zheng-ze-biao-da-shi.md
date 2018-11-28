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

re.compile\(\)函数可接受一个有用的标记：re.DOTALL，这使得表达式中的句点【.】可以匹配所有的字符，也包括换行符。

实例：

```
import re

str_pat=re.compile(r'/\*(.*?)\*/',re.DOTALL)
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

对于简单的情况，使用re.DOTALL标记就可以很好的完成工作。但是如果要处理及其复杂的模式，可以选择利用非捕获组定义在自己的表达式中，这样无需额外的标记也能正常工作。

