# 1、需求🙀

> 我们想对字符串中的文本做查找和替换。

# 2、解决方案😸

对于简单的文本模式，使用str.replace\(\)即可。

例如：

```
text='mark ，帅哥，18，183 帅，mark'
print(text.replace('18','19'))
print(text)
```

运行结果：

```
mark ，帅哥，19，193 帅，mark
mark ，帅哥，18，183 帅，mark
```

针对更为复杂的模式，可以使用re模块中的sub\(\)函数。

实例：将日期格式从“11/28/2018”改为“2018-11-28”

```
import re
text='今天是：11/28/2018'
print(re.sub(r'(\d+)/(\d+)/(\d+)',r'\3-\1-\2',text))
print(text)
```

结果：

```
今天是：2018-11-28
今天是：11/28/2018
```

sub\(\)的第一个参数是要匹配的模式，第二个参数是要替换的模式。类似的“\3”这样的反斜线加数字表示模式中捕获组的编号。

如果打算用相同的模式执行重复替换，可以考虑先将模式编译以获得更好的性能。

实例：

```
import re
text='今天是：11/28/2018'
datepat=re.compile(r'(\d+)/(\d+)/(\d+)')
print(datepat.sub(r'\3-\1-\2',text))
print(text)
```

结果：

```
今天是：2018-11-28
今天是：11/28/2018
```

对于更加复杂的情况，可以指定一个替换回调函数。

示例：

```
import re
from calendar import month_abbr
text='今天是：11/28/2018'
datepat=re.compile(r'(\d+)/(\d+)/(\d+)')

def change_date(m):
    mon_name=month_abbr[int(m.group(1))]
    return '{}  {}  {}'.format(m.group(3),mon_name,m.group(2))
print(datepat.sub(change_date,text))
print(text)
```

结果：

```
今天是：2018  Nov  28
今天是：11/28/2018
```

替换回调函数的输入参数是一个匹配对象，由match\(\)和find\(\)返回。用.group\(\)方法来提取匹配中特定的部分。该函数返回替换后的文本。

除了得到替换后的文本外，如果还想知道一共完成了多少次替换，可以使用re.subn\(\)。

示例：

```
import re
text='今天是：11/28/2018,昨天是11/27/2018'
datepat=re.compile(r'(\d+)/(\d+)/(\d+)')
new_text,n=datepat.subn(r'\3-\1-\2',text)
print(text)
print(new_text)
print(n)
```

结果：

```
今天是：11/28/2018,昨天是11/27/2018
今天是：2018-11-28,昨天是2018-11-27
2
```

# 3、分析😈

除了以上展示的sub\(\)调用之外，关于表达式的查找和替换并没有什么更多可说的了，最有技巧性的地方就是指定的正则表达式。。



