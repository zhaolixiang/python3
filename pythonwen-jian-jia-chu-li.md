# Python文件夹处理

### 1、创建文件夹

```
import os

os.mkdir("我是创建的文件夹")
```

### 2、获取当前目录

```
import os

print(os.getcwd())
```

结果：

```
/Users/zhaolixiang/Desktop/python/test1/文件夹
```

### 3、获取目录列表

```
import os

print(os.listdir("./"))
```

结果：

```
['1、创建文件夹.py', '我是创建的文件夹', '3、获取目录列表.py', '5、删除文件夹.py', '4、修改默认目录.py', '2、获取当前目录.py']
```

### 4、修改默认路径（切换路径）

```
import os
print(os.getcwd())
#返回上一目录
os.chdir("../")
print(os.getcwd())
```

结果：

```
/Users/zhaolixiang/Desktop/python/test1/文件夹
/Users/zhaolixiang/Desktop/python/test1
```

### 5、删除文件夹：

```
import os
print(os.listdir("./"))
os.rmdir("我是创建的文件夹")
print(os.listdir("./"))
```

运行结果：

```
['1、创建文件夹.py', '我是创建的文件夹', '3、获取目录列表.py', '5、删除文件夹.py', '4、修改默认目录.py', '2、获取当前目录.py']
['1、创建文件夹.py', '3、获取目录列表.py', '5、删除文件夹.py', '4、修改默认目录.py', '2、获取当前目录.py']
```



