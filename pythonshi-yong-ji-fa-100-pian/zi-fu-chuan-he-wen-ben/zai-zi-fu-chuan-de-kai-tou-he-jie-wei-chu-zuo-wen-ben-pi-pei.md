# 1、需求🙀

> 我们需要在字符串的开头和结尾处按照指定的文本模式做检查，例如检查文件的扩展名、URL协议类型等。

# 2、解决方案😸

有一种简单的方法可用来检查字符串的开头或结尾，只要使用str.startswith\(\)和str.endswith\(\)方法就可以了。

实例：

```
filename='mark.txt'
url='http://www.baidu.com'

print(filename.endswith('.txt'))
print(url.startswith('https:'))
```

运行结果：

```
True
False
```

需要需要同时针对多个选项做检查，只需要给startswith\(\)和endswith\(\)提供包含可能选项的元组即可：

```
url='http://www.baidu.com'
print(url.startswith(('https:','http:')))
```

结果：

```
True
```

> 注意，多个选项只能使用元组，如果使用列表或集合，请首先使用tuple\(\)将它们转化为元组，否则会报错。

# 3、分析😈

有时候我们也比较倾向于使用正则表达式作为替代方案，例如：

```
import re
url='http://www.baidu.com'
m=re.match('http:|http:|ftp',url)
print(m)

```

结果：

```
<re.Match object; span=(0, 5), match='http:'>
```



