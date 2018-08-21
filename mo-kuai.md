# Python模块

### 1、导入

> 导入整个模块：import 模块名
>
> 导入特定的函数：from 模块名 import 特定函数
>
> 使用as给函数重命名：from 模块名 import 特定函数 as 新名称
>
> 使用ad给模块冲命名 import 模块名 as  新名词
>
> 导入模块的所有函数：from 模块名 import  \*

### 2、Python解析器对模块位置的搜索顺序

> 1.当前目录。
>
> 2.搜索在shell变量PYTHONPATH下的所有目录
>
> 3.Python默认路径，例如UNIX下：，默认路径一般为：/usr/local/lib/python/

提示：模块搜索路径存储在system模块的sys.path变量中，该变量包含当前目录、PYTHONPATH、安装过程决定的默认路径

实例：

```
import sys
for item in sys.path:
    print(item)
```

结果：

```
/Users/zhaolixiang/Desktop/python/test1/模块
/Users/zhaolixiang/Desktop/python/test1
/Library/Frameworks/Python.framework/Versions/3.7/lib/python37.zip
/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7
/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/lib-dynload
/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages
/Applications/PyCharm.app/Contents/helpers/pycharm_matplotlib_backend
```

### 3、自定义模块、\_\__name_\_\_

> 自定义模块：就是自己写一个py文件啦，别紧张，没那么复杂
>
> \_\__name\_\_:通过判断这个变量是否等于\_\_main_\_\_，来判断该模块（py文件）是被别否认模块引用，还是自己直接调用，通过该方法可以在开发阶段进行单个模块测试。

实例：

MarkA.py

```
def funA():
    print("MarkA---funA")

#用来进行测试
if __name__=='__main__':
    print("MarkA测试调用")
    funA()
```

MarkB.py

```
import MarkA as markA

def funB():
    print("MarkB--funB")
    markA.funA()

if __name__=="__main__":
    funB()
```

运行MarkB的结果：

```
MarkB--funB
MarkA---funA
```

### 4、\_\__all_\_\_

> 当该变量列表存在时，只有在该列表中存在的属性或者方法才能被引用访问

实例：

AllA.py

```
__all__=["A","testA"]
class A:
    def testA(self):
        print("A---testA")
class B:
    def testB(self):
        print("B---testB")
def testA():
    print("testA")
def testB():
    print("testB")
```

AllB.py

```
from AllA import *
a=A()
a.testA()

testA()

'''
下面调用会出现异常，因为只有在__init__中的元素才能被导入
b=B()
b.testB()

testB()

'''
```

运行AllB.py，结果为：

```
A---testA
testA
```

### 5、包

> 在包含多个.py文件的文件夹中，新建一个\_\__init_\_\_.py文件，此时这个文件夹就成了包。
>
> 可以在这个文件中定义\_\__all_\_\_来决定包中哪些可以被其它模块导入。

### 6.模块发布

* ##### 打包前项目概况：

makeA.py:

```
def testA():
    print("testA")
```

makeB.py:

```
def testB():
    print("testB")
```

setup.py:

```
from distutils.core import setup
#打包的详细信息
setup(name="mark",version="1.0",description="mark's module",
      author="mark",py_modules=["makeA","makeB"])
```

![](/assets/Jietu20180812-220540.jpg)

* ##### 构建模块

```
python setup.py build
```

构建后的目录结构：

![](/assets/Jietu20180812-220758.jpg)

* ##### 生成发布压缩包

```
python setup.py sdist
```

执行后的目录结构：

![](/assets/Jietu20180812-220947.jpg)

dist目录下的mark-1.0.tar.gz就是打包后的文件

### 7、模块安装与使用

> 1、找到模块安装包
>
> 2、解压
>
> 3、进入文件夹
>
> 4、执行：python setup.py install
>
> 也可以指定安装路径：python setup.py install --prefix=安装路径
>
> 5、s会用from  import就可以完成引用使用了









