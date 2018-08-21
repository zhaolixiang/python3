# Python字典处理

### 1、根据键访问值

* ##### 普通访问

##### 实例：

```
info={"name":"Mark","age":18}
print("我的姓名：%s"%info["name"])
#如果没有指定的键，就会报错
print(info["sex"])
```

结果：

```
我的姓名：Mark
Traceback (most recent call last):
  File "/Users/zhaolixiang/Desktop/python/test1/字典/字典取值.py", line 4, in <module>
    print(info["sex"])
KeyError: 'sex'
```

* ##### get访问

> ##### 上面普通访问方法，如果找不到对于键，就会报错，而get访问，找不到就返回None，并且还可以设置当找不到时的默认值

实例：

```
info={"name":"Mark","age":18}
print("我的姓名：%s"%info.get("name"))
#如果没有指定的键，不会报错
print(info.get("sex"))
#设置默认值
print(info.get("sex","男"))
```

结果：

```
我的姓名：Mark
None
男
```

### 2、字典的遍历

* ##### 遍历key

##### 实例：

```
info={"name":"Mark","age":18}
for key  in info.keys():
    print(key)
```

##### 结果：

```
name
age
```

* ##### 遍历value

##### 实例：

```
info={"name":"Mark","age":18}
for value  in info.values():
    print(value)
```

##### 结果：

```
Mark
18
```

* ##### 遍历元素

##### 实例：

```
info={"name":"Mark","age":18}
for item  in info.items():
    print(item)
```

##### 结果：

```
('name', 'Mark')
('age', 18)
```

* ##### 遍历键值对

##### 实例：

```
info={"name":"Mark","age":18}
for key,value  in info.items():
    print("%s=%s"%(key,value))
```

##### 结果：

```
name=Mark
age=18
```

### 3、修改元素

> 通过key对指定元素进行修改

实例：

```
info={"name":"Mark","age":18}
print("修改前：",info)
info["age"]=19
print("修改后：",info)
```

结果：

```
修改前： {'name': 'Mark', 'age': 18}
修改后： {'name': 'Mark', 'age': 19}
```

### 4、添加元素

> 字典变量名\[key\]=value，如果key存在就是修改，不存在就添加

实例：

```
info={"name":"Mark","age":18}
print("添加前：",info)
info["age"]=19
print("这个不是添加，只是修改值：",info)
info["sex"]="男"
print("添加后：",info)
```

结果：

```
添加前： {'name': 'Mark', 'age': 18}
这个不是添加，只是修改值： {'name': 'Mark', 'age': 19}
添加后： {'name': 'Mark', 'age': 19, 'sex': '男'}
```

### 5、删除元素

> del  ：删除单个元素或者直接删除这个字典变量定义
>
> clear：清空字典

实例：

```
info={"name":"Mark","age":18}
print("del前：",info)
del info["age"]
print("del单个元素：",info)

info={"name":"Mark","age":18}
info.clear()
print("clear清空字典：",info)



info={"name":"Mark","age":18}
del info
print("del删除字典变量：",info)
```

结果：

```
del前： {'name': 'Mark', 'age': 18}
del单个元素： {'name': 'Mark'}
clear清空字典： {}
Traceback (most recent call last):
  File "/Users/zhaolixiang/Desktop/python/test1/字典/字典del.py", line 14, in <module>
    print("del删除字典变量：",info)
NameError: name 'info' is not defined
```

### 6、其它操作

| 操作 | 解释 |
| :--- | :--- |
| len\(\) | 返回字典中键值对个数 |
| keys\(\) | 返回一个包含字典所有键的列表 |
| values\(\) | 返回一个包含字典所有值得列表 |
| items\(\) | 返回一个包含字典所有元组（键、值）的列表 |
| has\_key\(key\) | 如果字典中存在key则返回true，否则返回false |



