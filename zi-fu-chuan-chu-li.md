
---

# python字符串处理

### 1、修改字符串的大小写

| 方法 | 含义 |
| :--- | :--- |
| title\(\) | 将每个单词首字母大写 |
| upper\(\) | 将每个字母都大写 |
| lower\(\) | 将每个字母都小写 |
| capitalize\(\) | 将字符串首字母大写,其余字符小写 |

实例展示：

```
name="zhao li Xiang"
print("单词首字母大写前：%s"%name)
name.title()
print("单词首字母大写后（不用name接收）：%s"%name)
name=name.title()
print("单词首字母大写后（用name接收）：%s"%name)

print("*"*50)

name="zhao li Xiang"
print("所有字母都大写前：%s"%name)
name.upper()
print("所有字母都大写后（不用name接收）：%s"%name)
name=name.upper()
print("所有字母大写后（用name接收）：%s"%name)

print("*"*50)

name="zhao li Xiang"
print("所有字母都小写前：%s"%name)
name.lower()
print("所有字母都小写后（不用name接收）：%s"%name)
name=name.lower()
print("所有字母都小写后（用name接收）：%s"%name)

print("*"*50)

name="zhao li Xiang"
print("字符串首字母大写，其它字符小写前：%s"%name)
name.capitalize()
print("字符串首字母大写，其它字符小写后（不用name接收）：%s"%name)
name=name.capitalize()
print("字符串首字母大写，其它字符小写后（用name接收）：%s"%name)
```

控制台打印结果

```
单词首字母大写前：zhao li Xiang
单词首字母大写后（不用name接收）：zhao li Xiang
单词首字母大写后（用name接收）：Zhao Li Xiang
**************************************************
所有字母都大写前：zhao li Xiang
所有字母都大写后（不用name接收）：zhao li Xiang
所有字母大写后（用name接收）：ZHAO LI XIANG
**************************************************
所有字母都小写前：zhao li Xiang
所有字母都小写后（不用name接收）：zhao li Xiang
所有字母都小写后（用name接收）：zhao li xiang
**************************************************
字符串首字母大写前：zhao li Xiang
字符串首字母大写后（不用name接收）：zhao li Xiang
字符串首字母大写后（用name接收）：Zhao li xiang
```

不难看出：无论是上面三个方法中的哪一个，都只是返回改变后的结果，对操作的字符串对象没有任何影响！

> 将所有字母大写或者小写，可用来判断用户输入是使用，可以做到相应的人性化关怀（大小写都可以）

### 2、合并（拼接字符串）

使用+号来合并

```
print("Mark"+"帅哥🤗")
```

打印结果：

```
Mark帅哥🤗
```

### 3、使用制表符或换行符来添加空白

\t:制表符

\n:换行符

```
print("Mark\t帅哥")
print("Mark\n帅哥")
```

控制台打印结果：

```
Mark 帅哥
Mark
帅哥
```

### 4、删除空白

lstrip\(\):删除字符串开头空白

rstrip\(\)：删除字符串尾部空白

strip\(\):删除字符串前后字空白

> 注意：上面两个函数都只是返回删除空白后的字符串，对原有字符串没有改变

实例：

```
name=" Mark "
name.lstrip()
print("*"+name+"*")
name.rstrip()
print("*"+name+"*")
print("*"+name.lstrip().rstrip()+"*")
print("*"+name.strip()+"*")
```

打印结果：

```
* Mark *
* Mark *
*Mark*
*Mark*
```

### 5、下标与切片

下标：字符串可以看出是一个字符数组，而在这个数组的位置就是对应字符的下标

切片：是对操作对象截取一部分的操作。字符串、列表、元组都支持切片

> 切片的语法：\[起始下标:结束下标:步长\]

如果有字符串“abcdef”，在内存中实际存储如下：

![](/assets/import123.png)

可以通过下标获取指定字符：

```
name="abcdef"
print(name[0])
print(name[1])
print(name[2])
print(name[3])
```

打印结果：

```
a
b
c
d
```

切片操作：

```
name="abcdef"
print(name[:3])
print(name[1:3])
print(name[3:])
print(name[1:5:2])
#从第二个字符到最后第二个字符
print(name[1:-1])
#倒序
print(name[::-1])
```

打印结果：

```
abc
bc
def
bd
bcde
fedcba
```

### 6、find、rfind：查找指定字符串是否存在于目标字符串

> 语法：目标字符串.find\(需要查询的指定字符串\[开始查询的下标,结束查询的下标\]\)：从左边开始查找
>
> 目标字符串.find\(需要查询的指定字符串\[开始查询的下标,结束查询的下标：从右边开始查找
>
> 存在就会返回下标，不存在就返回-1

实例：

```
name="abcdefg"
print(name.find("cd"))
print(name.rfind("cd"))
print(name.find("gg"))
print(name.rfind("gg"))
```

控制台打印结果：

```
2
2
-1
-1
```

### 7、index、rindex：查找指定字符串是否存在于目标字符串

> 与find的不同点是如果没有查询到，index方法会报错

实例：

```
name="abcdefg"
print(name.index("cd"))
print(name.rindex("cd"))
print(name.index("gg"))
print(name.rindex("gg"))
```

打印结果：

```
2
Traceback (most recent call last):
2
File "/Users/zhaolixiang/Desktop/python/test1/venv/字符串index.py", line 4, in <module>
print(name.index("gg"))
ValueError: substring not found
```

### 8、count：返回指定字符串在目标字符串出现的次数

> 语法：目标字符串.count\(指定字符串\[,开始下标,结束下标\]\)

实例：

```
name="mark mark mark"
print(name.count("ar"))
print(name.count("ar",0,5))
print(name.count("ar",5))
```

打印结果：

```
3
1
2
```

### 9、replace：替换字符串

> 语法：mystr.replace\(str1,str2\[,count\]\)
>
> 将mystr中的str1替换为str2，如果count指定，则替换次数不超过count次。
>
> 这个方法也是只影响返回值，不对mystr对象进行改变。

实例：

```
name="mark mark mark"
print(name.replace("ar","br"))
print(name.replace("ar","br",0))
print(name.replace("ar","br",1))
```

打印结果：

```
mark mark mark
mbrk mbrk mbrk
mark mark mark
mbrk mark mark
```

### 10、split分割字符串

> 语法：mystr.split\(str\[,count\]\)
>
> 把mystr依靠str进行分割，如果count指定，则最多分割count个字符串

实例：

```
name="mark mark mark"
print(name.split("ar"))
print(name.split("ar",0))
print(name.split("ar",2))
```

打印结果：

```
['m', 'k m', 'k m', 'k']
['mark mark mark']
['m', 'k m', 'k mark']
```

### 11、startswith、endswith判断是否以指定字符串开发或结束

实例：

```
name="Mark 帅哥"
print(name.startswith("Mar"))
print(name.startswith("mar"))
print(name.endswith("哥"))
print(name.endswith("帅"))
```

控制台打印结果：

```
True
False
True
False
```

### 12、ljust、rjust、center：返回一个元字符左对齐或右对齐或居中，用空白填充至指定宽度的字符串

实例：

```
name="mark"
print("*"+name.ljust(2)+"*")
print("*"+name.ljust(6)+"*")
print("*"+name.rjust(6)+"*")
print("*"+name.center(6)+"*")
```

打印结果：

```
*mark*
*mark *
* mark*
* mark *
```

### 13、partition、rpartition：将目标字符串安装指定字符串分割为三部分

实例：

```
name="hello world world mark"
print(name.partition("world"))
print(name.rpartition("world"))
```

打印结果：

```
('hello ', 'world', ' world mark')
('hello world ', 'world', ' mark')
```

### 14、splitlines按行分割字符串

实例：

```
name="hello\nworld\nworld\nmark"
print(name.splitlines())
```

打印结果：

```
['hello', 'world', 'world', 'mark']
```

### 15、isalpha：判断字符串是否都是字符

实例：

```
print("mark".isalpha())
print("mark123".isalpha())
print("mark帅哥".isalpha())
print("mark 帅哥".isalpha())
```

打印结果：

```
True
False
True
False
```

### 16、isdigit：判断字符串是否都是数字

实例：

```
print("mark".isdigit())
print("123".isdigit())
print("123帅哥".isdigit())
```

打印结果：

```
False
True
False
```

### 17、isalnum：判断字符串是否都是字符或数字

实例：

```
print("mark".isalnum())
print("mark123".isalnum())
print("123".isalnum())
print("mark 帅哥".isalnum())
```

打印结果：

```
True
True
True
False
```

### 18、isspace：判断字符串是否只包含空格

实例：

```
print("mark".isspace())
print("mark 123".isspace())
print(" ".isspace())
```

打印结果：

```
False
False
True
```

### 19、join：每个字符后面插入指定字符

实例：

```
list=["my","name","is","mark"]
print(" ".join(list))
print("_".join(list))
```

打印 结果：

```
my name is mark
my_name_is_mark
```



