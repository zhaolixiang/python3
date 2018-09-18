Python中的所有对象都享受着【头等】（first class）待遇，称作第一类对象。这意味着所有能用标识符命名的对象都具有平等身份。**这还意味着所有能被命名的对象都可以当做数据处理。**

例如，下面给出了一个包含两个值得简单字典：

```
items={
'number':42,
'text':'Hello World'
}
```

给这个字典添加一些特殊的项，便可以看到对象的第一类本质，例如：

```
items["func"]=abs    #添加abs()函数
import math
items["mod"]=math    #添加一个模块
items["error"]=ValueError  #添加一个异常类型
nums=[1,2,3,4]
items["append"]=nums.append  #添加另一个对象的一个方法
```



