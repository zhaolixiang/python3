# 1、需求🙀

> 做一个有限个数的历史记录。

# 2、解决方案😸

> deque\(maxlen=N\)，创建一个固定长度的队列，当有新记录加入并且队列已满时会自动移除最老的那条记录。

代码：

```
from collections import deque

q=deque(maxlen=3)

q.append(1)
q.append(2)
q.append(3)
print(q)
q.append(4)
print(q)
q.append(5)
print(q)
```

结果：

```
deque([1, 2, 3], maxlen=3)
deque([2, 3, 4], maxlen=3)
deque([3, 4, 5], maxlen=3)
```

如果不指定队列的大小，也就得到了一个无界限的队列，可以在两端执行添加和弹出操作，

代码：

```
from collections import deque

q=deque()
q.append(1)
q.append(2)
q.append(3)
q.append(4)
print(q)
q.appendleft(5)
print(q)
print(q.pop())
print(q)
print(q.popleft())
print(q)
```

结果：

```
deque([1, 2, 3, 4])
deque([5, 1, 2, 3, 4])
4
deque([5, 1, 2, 3])
5
deque([1, 2, 3])
```



