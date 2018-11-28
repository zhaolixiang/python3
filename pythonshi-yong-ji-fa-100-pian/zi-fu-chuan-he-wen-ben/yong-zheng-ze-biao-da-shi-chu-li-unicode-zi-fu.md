# 1、需求🙀

> 我们正在使用正则表达式处理文本，但是需要考虑处理Unicode字符。

# 2、解决方案😸

默认情况下re模块已经对某些Unicode字符类型有了基本的认识。例如，\d已经可以匹配任意Unicode数字字符了:

```
import re
num=re.compile('\d+')

str1='123'
str2='\u0661\u0662\u0663'

print(num.match(str1))
print(num.match(str2))
```

结果：

```
<re.Match object; span=(0, 3), match='123'>
<re.Match object; span=(0, 3), match='١٢٣'>
```

如果需要在模式字符串中包含指定的Unicode字符，可以针对Unicode字符使用转义序列（例如\uFFFF或\UFFFFFFF）。比如，这里有一个正则表达式在多个不同的阿拉伯代码页中匹配所有的字符：

```
re.compile('[\u0600-\u06ff\u0750-\u077f\u08a0-\u08ff]+')
```

当执行匹配和搜索操作时，一个好主意是首先将所有的文本都统一表示为标准模式（见上一节）。但是，同样重要的是需要注意一些特殊情况，例如，当不区分大小写的匹配和大写转换匹配联合起来时，考虑会出现什么行为：

```

```

1

1

1

1

1

1

1

1

11

# 3、分析😈



