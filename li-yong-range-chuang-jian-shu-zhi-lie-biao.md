# Python创建数值列表

### 1、range

> range\(\)可以产生一些列数值
>
> 语法：range\(开始下标\(包括\),结束下标\(不包括\)\[,步长\]\)

实例：

```
for value in range(1,5):
    print(value)

print("*"*30)

for value in range(1,5,2):
    print(value)
```

结果：

```
1
2
3
4
******************************
1
3
```

### 2、利用list\(\)将range产生的数值转化为列表

实例：

```
nums=list(range(1,5))
print(nums)
print("*"*30)

nums=list(range(1,5,2))
print(nums)
```

结果：

```
[1, 2, 3, 4]
******************************
[1, 3]
```

### 3、列表解析：快速生成数值列表

> 语法实例：list=\[value\*\*2 for  value in range\(1,11\)\]

实例：

```
list=[value**2 for value in range(1,5)]
print(list)
```

结果：

```
[1, 4, 9, 16]
```



