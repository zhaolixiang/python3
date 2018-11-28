# 1、需求🙀

> 我们需要以不区分大小写的方式在文本中进行查找，可能还需要做替换。

# 2、解决方案😸

要进行不区分大小写的文本操作，我们需要使用re模块并且对各种操作都要加上re.IGNORECASE标记。

示例：

```
import re
text='Mark is a handsome guy and mark is only 18 years old.'
result1=re.findall('mark',text,flags=re.IGNORECASE)
result2=re.sub('mark','python',text,flags=re.IGNORECASE)

print(result1)
print(result2)
```

结果：

```
['Mark', 'mark']
python is a handsome guy and python is only 18 years old.
```

上面例子揭示了一种局限，就是虽然名字从【mark】替换为【python】，但是大小写并不吻合，例如第一个人名替换后应该也是大写：【Pyhton】。

如果想要修正这个问题，需要用到一个支撑函数，实例如下：

```
import re
text='Mark is a handsome guy and mark is only 18 years old.MARK'

def matchcase(word):
    def replace(m):
        #re.sub会将匹配到的对象，循环调用replace方法传入
        print(m)
        #获取匹配的文本
        text=m.group()
        if text.isupper():
            #如果文本全部是大写，就返回word的全部大写模式
            return word.upper()
        elif text.islower():
            # 如果文本全部是小写，就返回word的全部小写模式
            return word.lower()
        elif text[0].isupper():
            #如果文本是首字母大写，就返回word的首字母大写模式
            return word.capitalize()
        else:
            #其他情况，直接返回word
            return word
    return replace

result=re.sub('mark',matchcase('python'),text,flags=re.IGNORECASE)

print(result)


```

1

1

1

1

# 3、分析😈



