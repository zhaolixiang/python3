# 1、需求🙀

> 我们想要按照特定的文本模式进行匹配或查找。

# 2、解决方案😸

如果想要匹配的只是简单的文字，那么通常只需要用基本的字符串方法就可以了，比如str.find\(\)、str.endswith\(\)、str.startswith\(\)或类似函数。

示例：

```
text='mark ，帅哥，18，183 帅，mark'
print(text=='mark')
print(text.startswith('mark'))
print(text.startswith('mark'))
print(text.find('帅哥'))
```

结果：

```
False
True
True
6
```

如果更为复杂的匹配则需要使用正则表达式以及re模块。为了说明使用正则表达式的基本流程，假设我们想匹配以数字形式构成的日期，比如"11/27/2018"。示例如下：

```
import re
text1='11/27/2018'
text2='Nov 27, 2018'
if re.match(r'\d+/\d+/\d',text1):
    print('符合模型：数字/数字/数字')
else:
    print('不符合模型：数字/数字/数字')

if re.match(r'\d+/\d+/\d',text2):
    print('符合模型：数字/数字/数字')
else:
    print('不符合模型：数字/数字/数字')
```

运行结果：

```
符合模型：数字/数字/数字
不符合模型：数字/数字/数字
```

如果打算针对同一模型做多次匹配，那么通常会先将正则表达式模式预编译成一个模式对象。

例如：

```
import re
text1='11/27/2018'
text2='Nov 27, 2018'
datepat=re.compile(r'\d+/\d+/\d')
if datepat.match(text1):
    print('符合模型：数字/数字/数字')
else:
    print('不符合模型：数字/数字/数字')

if datepat.match(text2):
    print('符合模型：数字/数字/数字')
else:
    print('不符合模型：数字/数字/数字')
```

结果：

```
符合模型：数字/数字/数字
不符合模型：数字/数字/数字
```

match\(\)方法总是尝试在字符串的开头找到匹配项。如果想针对整个文本搜索出所有的匹配项，那么就应该使用findall\(\)方法，例如：

```

```

1

1

# 3、分析😈



