# 1、需求🙀

> 我们需要以不区分大小写的方式在文本中进行查找，可能还需要做替换。

# 2、解决方案😸

要进行不区分大小写的文本操作，我们需要使用re模块并且对各种操作都要加上re.IGNORECASE标记。

示例：

```
import re
text='Mark is a handsome guy and mark is only 18 years old.'
result1=re.findall('mark',text,flags=re.IGNORECASE)
result2=re.sub('mark','lx',text,flags=re.IGNORECASE)

print(result1)
print(result2)

```

结果：

```

```

1

1

1

1

1

# 3、分析😈



