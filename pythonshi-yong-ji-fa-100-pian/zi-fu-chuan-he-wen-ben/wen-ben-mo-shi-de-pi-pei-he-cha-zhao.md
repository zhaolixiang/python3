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
if re.match(r'\d+/\d+/\d+',text1):
    print('符合模型：数字/数字/数字')
else:
    print('不符合模型：数字/数字/数字')

if re.match(r'\d+/\d+/\d+',text2):
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
datepat=re.compile(r'\d+/\d+/\d+')
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
import re
text='今天是 11/27/2018，昨天是11/26/2018'
datepat=re.compile(r'\d+/\d+/\d+')
print(datepat.findall(text))
```

运行结果：

```
['11/27/2018', '11/26/2018']
```

当定义正则表达式时，我们常会将部分模式用括号包起来的方式引入捕获组，捕获组通常简化后续对匹配文本的处理，因为每个组的内容都可以单独提取出来。findall\(\)方法搜索整个文本并找出所有的匹配项然后将它们以列表的形式返回。如果想以迭代的方式找出匹配项，可以使用finditer\(\)方法。

例如：

```
import re
#加入捕获组
datepat=re.compile(r'(\d+)+/(\d+)+/(\d+)')
m=datepat.match('11/27/2018')
print(m.group(0))
print(m.group(1))
print(m.group(2))
print(m.group(3))
print(m.groups())
month,day,year=m.groups()
print(month)
print(day)
print(year)

print('*'*20)

text='今天是 11/27/2018，昨天是11/26/2018'
for month,day,year in datepat.findall(text):
    print('{}-{}-{}'.format(year,month,day))

print('*'*20)
for m in datepat.finditer(text):
    print(m.groups())
```

结果：

```
11/27/2018
11
27
2018
('11', '27', '2018')
11
27
2018
********************
2018-11-27
2018-11-26
********************
('11', '27', '2018')
('11', '26', '2018')
```

# 3、分析😈

本节主要介绍了re模块对文本匹配和搜索的基本功能，首先用re.compile\(\)对模式进行编译，然后使用想match\(\)、findall\(\)、finditer\(\)这样的方法做匹配和搜索。

当指定模式时我们通常会使用原始字符串，例如：

```
r'(\d+)/(\d+)/(\d+)'
```

这样的字符串不会对反斜字符转义，这在正则表达式中非常有用。否则，我们需要用双反斜杠线来标识一个单独的'\'，例如：

```
'(\\d+)/(\\d+)/(\\d+)'
```

请注意match\(\)方法只会从字符串的开头开始匹配，有可能出现的匹配的结果并不是你想要的，例如：

```

```

结果：

```

```

