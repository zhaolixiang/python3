# 1、需求🙀

> 我们正在同Unicode字符串打交道，但需要确保所有的字符串都拥有相同的底层表示。

# 2、解决方案😸

在Unicode中，有些特定的字符可以被表示成多种合法的代码点序列。为了说明这个问题，请看下面示例：

```
str1='\u00f1'
str2='n\u0303'
str3='\u0303'

print(str1)
print(str2)
print(str3)

print(str1==str2)
print(len(str1))
print(len(str2))
```

运行结果：

```
ñ
ñ
̃
False
1
2
```

这里的文本ñ是以两种形式呈现:

* 第一种使用的是字符ñ的【全组成】形式：U+00F
* 第二种使用的是拉丁字母n紧跟着一个“~”，【组合】而成的字符：U+0303

对于一个比较字符串的程序来说，同一个文本拥有多种不同的表示形式是个大问题。为了解决这个问题，应该先将文本同一表示为规范形式，这可以通过unicodedata模块来完成。

实例：

```
import unicodedata
str1='\u00f1'
str2='n\u0303'

t1=unicodedata.normalize('NFC',str1)
t2=unicodedata.normalize('NFC',str2)

print(t1)
print(t2)

print(t1==t2)

print(len(t1))
print(len(t2))

print('*'*10)

t3=unicodedata.normalize('NFD',str1)
t4=unicodedata.normalize('NFD',str2)
print(t3)
print(t4)

print(t3==t4)

print(len(t3))
print(len(t4))
```

结果：

```
ñ
ñ
True
1
1
**********
ñ
ñ
True
2
2
```

normalize\(\)的第一个参数指定了字符串应该如何完成规范表示。

* NFC表示字符串应该是【全组成】的（即，如果可能的话使用单个代码点）。
* NFD表示应该使用【组合】字符，每个字符应该是能完全分解的。

Python还支持NFKC和NFKD的规范表示形式，它们为处理特定类型的字符增加额额外的兼容功能。

实例：

```
import unicodedata
str='\ufb01'

print(str)
print(len(str))
print('*'*10)

t_nfd=unicodedata.normalize('NFD',str)
t_nfkd=unicodedata.normalize('NFKD',str)
t_nfc=unicodedata.normalize('NFC',str)
t_nfkc=unicodedata.normalize('NFKC',str)

print(t_nfd)
print(len(t_nfd))
print('*'*10)

print(t_nfkd)
print(len(t_nfkd))
print('*'*10)

print(t_nfc)
print(len(t_nfc))
print('*'*10)

print(t_nfkc)
print(len(t_nfkc))
```

运行结果：

```
ﬁ
1
**********
ﬁ
1
**********
fi
2
**********
ﬁ
1
**********
fi
2
```

1

1

1

# 3、分析😈



