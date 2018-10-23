# 1、需求🙀

> 我们的代码是通过位置（即索引或下标）来访问列表会元组的，但有时候这会让代码变得有些难以阅读。我们希望可以通过名称来访问元素，以此减少结构中对位置的依赖性。

# 2、解决方案😸

相比普通的元组，collections.namedtuple\(\)\(命名元组\)只增加了极少的开销就提供了这些便利。实际上collections.namedtuple\(\)是一个工厂方法，它返回的是Python中标准元组类型的子类。我们提供给它一个类型名称以及相应的字段，它就返回一个可实例化的类、为你已经定义好的字段传入值等。

```
from collections import namedtuple
Subscriber=namedtuple('Subsciber',['addr','joined'])
sub=Subscriber("1782980833@qq.com","2018-10-23")

print(sub)
print(sub.addr)
print(sub.joined)
```

结果：

```
Subsciber(addr='1782980833@qq.com', joined='2018-10-23')
1782980833@qq.com
2018-10-23
```

大幅度

# 3、分析😈



