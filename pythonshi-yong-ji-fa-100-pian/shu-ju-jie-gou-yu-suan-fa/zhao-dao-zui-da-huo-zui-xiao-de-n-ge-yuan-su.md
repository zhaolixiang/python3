# 1、需求🙀

> 我们想在某个集合中找出最大或最小的N个元素

# 2、解决方案😸

> heapq模块中有两个函数：nlargest\(\)和nsmallest\(\)

代码：

```
import heapq

nums=[1,444,66,77,34,67,2,6,8,2,4,9,556]
print(heapq.nlargest(3,nums))
print(heapq.nsmallest(3,nums))
```

结果：

```
[556, 444, 77]
[1, 2, 2]
```

这个两个函数都可以接受一个参数key，从而允许他们可以工作在更加复杂的数据结构上：

代码：

```

```

结果：

```

```



