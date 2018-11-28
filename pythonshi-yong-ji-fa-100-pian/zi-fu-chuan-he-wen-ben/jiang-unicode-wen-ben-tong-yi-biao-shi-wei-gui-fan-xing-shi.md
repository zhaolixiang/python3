# 1、需求🙀

> 我们正在同Unicode字符串打交道，但需要确保所有的字符串都拥有相同的底层表示。

# 2、解决方案😸

在Unicode中，有些特定的字符可以被表示成多种合法的代码点序列。为了说明这个问题，请看下面示例：

```
str1='\u00f1'
str2='n\u0303'

print(str1)
print(str2)

print(str1==str2)
print(len(str1))
print(len(str2))

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

# 3、分析😈



