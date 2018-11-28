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

```

1

1

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



