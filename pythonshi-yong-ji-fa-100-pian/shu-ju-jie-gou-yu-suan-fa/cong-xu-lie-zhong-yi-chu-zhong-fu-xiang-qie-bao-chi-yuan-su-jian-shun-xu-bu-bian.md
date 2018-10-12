# 1、需求🙀

> 我们想去除序列出现的重复元素，但仍然保持剩下的元素的顺序不变。

# 2、解决方案😸

如果序列中的值是可哈希的（hashable），那么这个问题可以通过使用集合和生成器轻松解决。

```
def dedupe(items):
    seen=set()
    for item in items:
        if item not in seen:
            yield item
            seen.add(item)

a=[1,2,3,1,9,1,5,10]
print(list(dedupe(a)))
```

运行结果：

```
[1, 2, 3, 9, 5, 10]
```



