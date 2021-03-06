python是一门弱类型的语言，很多时候我们可能不清楚函数参数类型或者返回值类型，很有可能导致一些类型没有指定方法，typing模块可以很好的解决这个问题。

> 该模块加入并不会影响程序的运行，不会报正式的错误，只有提醒。
>
> typing模块只有在python3.5以上的版本中才可以使用，pycharm目前支持typing检查

# 一、typing模块的作用

1. 类型检查，防止运行时出现参数和返回值类型不符合的问题。
2. 作为开发文档附件说明，方便使用者调用时传入和返回参数类型。

# 二、typing模块的常用方式

先看实例代码：

```
from typing import List,Tuple,Dict
def add(a:int,string:str,f:float,b:bool)->Tuple[List,Tuple,Dict,bool]:
    list1=list(range(a))
    tup=(string,string,string)
    d={"a":f}
    bl=b
    return list1,tup,d,bl

if __name__ == '__main__':
    print(add(5,'mark',183.1,False))
```

运行结果：

```
([0, 1, 2, 3, 4], ('mark', 'mark', 'mark'), {'a': 183.1}, False)
```

说明：

1. 在传入参数时，通过“参数名:类型”的形式声明参数的类型；
2. 返回结果通过“-&gt;结果类型”的形式声明结果的类型
3. 在调用的时候如果参数的类型不正确pycharm会有提醒，但不会影程序的运行。
4. 对于如list列表等，还可以规定更加具体一些，如“-&gt;List\[str\]”，规定返回的是列表，并且元素是字符串。

现在对上面代码进行修改，可以看到pycharm背景变黄色区域，就是错误类型提醒：

# ![](/assets/Jietu20181122-140418.jpg)三、typing常用的类型

1. int,long,float：整型，长整型，浮点型
2. bool,str：布尔型，字符串类型
3. List,Tuple,Dict,Set：列表，元组，字典，集合
4. Iterable,Iterator：可迭代器，迭代器类型
5. Generator：生成器类型

# 四、typing支持可能的多种类型

由于python天生支持多态，迭代器中的元素可能有多种。

代码实例：

```
from typing import List, Tuple, Dict


def add(a: int, string: str, f: float, b: bool or str) -> Tuple[List, Tuple, Dict, str or bool]:
    list1 = list(range(a))
    tup = (string, string, string)
    d = {"a": f}
    bl = b
    return list1, tup, d, bl


if __name__ == '__main__':
    print(add(5, 'mark', 183.1, False))
    print(add(5, 'mark', 183.1, 'False'))

```

运行结果（跟不用typing无异）：

```
([0, 1, 2, 3, 4], ('mark', 'mark', 'mark'), {'a': 183.1}, False)
([0, 1, 2, 3, 4], ('mark', 'mark', 'mark'), {'a': 183.1}, 'False')
```















































# 



