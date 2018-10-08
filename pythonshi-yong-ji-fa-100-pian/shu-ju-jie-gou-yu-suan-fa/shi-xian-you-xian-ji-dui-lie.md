# 1、需求🙀

> 我们想要实现一个队列，它能够以给定的优先级来对元素排序，且每次pop操作时都会返回优先级最高的那个元素

# 2、解决方案😸

> 利用heapq模块实现

代码：

```
import heapq

#利用heapq实现一个简答的优先级队列
class PriorityQueue:
    def __init__(self):
        self._queue=[]
        self._index=0
    def push(self,item,priority):
        heapq.heappush(self._queue,(-priority,self._index,item))
        self._index+=1
    def pop(self):
        return heapq.heappop(self._queue)[-1]

class Item:
    def __init__(self,name):
        self.name=name

    def __repr__(self):
        return 'Item({!r})'.format(self.name)

if __name__ == '__main__':
    q=PriorityQueue()
    q.push(Item('foo'),1)
    q.push(Item('bar'),5)
    q.push(Item('spam'),4)
    q.push(Item('grok'),1)

    print(q.pop())
    print(q.pop())
    print(q.pop())
    print(q.pop())


```

运行结果：

```
Item('bar')
Item('spam')
Item('foo')
Item('grok')
```



