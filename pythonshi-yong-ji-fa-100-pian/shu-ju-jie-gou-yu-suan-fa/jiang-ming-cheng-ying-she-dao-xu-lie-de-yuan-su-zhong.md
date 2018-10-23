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

print(len(sub))
addr,joined=sub
print(addr)
print(joined)
```

结果：

```
Subsciber(addr='1782980833@qq.com', joined='2018-10-23')
1782980833@qq.com
2018-10-23
2
1782980833@qq.com
2018-10-23
```

尽管namedtuple的实例看起来就像一个普通的类实例，但它的实例与普通的元组是可互换的，而且支持所有普通元组所支持的操作。

命名元组的主要作用在于将代码同它所控制的元素位置间解耦。所以，如果从数据库调用中得到了一个大型的元组列表，而且通过元素的位置来访问元素，那么假如在表单中新增了一列数据，那么代码就会崩溃，但如果首先将返回的元组转换为命名元组，就不会出现问题。

为了说明这个问题，下面有一些使用普通元组的代码：

```
def compute_cost(records):
    total=0.0
    for rec in records:
        total+=rec[1]*rec[2]
    return total
```

通过位置来引用元素常常使得代码的表达力不够强，而且也很依赖于记录的具体结构。

下面是使用命名元组的版本：

```

```

# 3、分析😈



