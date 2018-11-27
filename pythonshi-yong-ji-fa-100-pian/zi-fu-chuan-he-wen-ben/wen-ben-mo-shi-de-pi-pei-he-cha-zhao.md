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

1

1

1

1

1

1

1

# 3、分析😈



