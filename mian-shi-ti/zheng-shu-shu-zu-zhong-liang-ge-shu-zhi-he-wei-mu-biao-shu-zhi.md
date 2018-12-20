> 给定一个整数组，例如：
>
> nums=\[2,7,11,15\]
>
> 再给定一个目标值，例如：9，假设整数值仅存在一组值使得其中两个数的和等于目标值，例如：
>
> 2+7=9，写出函数，找出数组中和为目标值的两个数的下标，例如：\[0,1\]。

代码：

```
def two_sun(nums,target):
    d={}
    for i,num in enumerate(nums):
        if target-num in d:
            return [d[target-num],i]
        d[num]=i

if __name__ == '__main__':
    print(two_sun([2,7,11,14],13))
```

运行结果：”

```
[0, 2]
```



