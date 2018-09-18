在程序进行像a=b这样的赋值时，就会创建一个对b的新引用。对于像数字和字符串这样的不可变对象，这种赋值实际上创建了b的一个副本，然而，对可变对象\(如列表、字典\)来说，这样赋值的效果大不一样，例如：

```
a=[1,2,3,4]
b=a              #b是对a的引用
b is a  #结果为True
b[2]=100
print(a)  #返回[1,2,100,4]
```

本例中，a和b引用的是同一个对象，修改其中任意一个变量都会影响到另一个。为了避免这种情况，必须创建对象的副本而不是创建新引用。

对于像列表和字典这样的容器对象，可以使用两种复制操作：浅赋值和深复制。

浅复制将创建一个新对象，但它包含的是对原始对象中包含的项的引用，例如：

```
a=[1,2,[3,4]]
b=list(a)   #创建a的一个浅复制
b is a      #返回false

b.append(100)  #给b追加一个元素
print(b)  #返回结果为[1,2,[3,4],100]
print(a)  #返回结果 为[1,2,[3,4]]

b[2][0]=88  #修改b中的一个元素

print(b)  #返回结果为：[1,2,[88,4]]
print(a)  #返回结果为：[1,2,[88,4]]
```

如上例所示，浅复制是说两个对象不是同一个对象，但是，两个对象包含的共同对象共享。

深复制将创建一个新对象，并且递归地复制它包含的所有对象。使用copy.deepcopy\(\)实现深拷贝。
