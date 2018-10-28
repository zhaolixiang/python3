# 2、save-update级联

save-update级联是指当一个父对象被新增到session中时，该对象当时关联的子对象也自动被新增到session中。

通过如下代码建立一个父对象class和两个子对象students1与students2

```
class_=Class()
student1,student2=Student(),Student()
class_.students.append(student1)
class_.students.append(student2)
```

如果父子级联关系包含save-update，则只需要将父对象保存到session中，子对象会自动被保存。

```
session.add(class_)
if student1 in session:
    print("The student1 has been added too!")
```

上面代码将会打印："The student1 has been added too!"

> 技巧：”in“语句可以判断某对象是否被关联到了session中。已被关联的对象在session被commit时会被写入到视频库中。

即使父对象已经被新增到session中，新关联的子对象仍然可以被添加：

```
class_=Class()
session.add(class_)
students3=Student()
if student3 in session:
    print("The student3 is added before append to the class_!")
class_.students.append(students)
if student1 in session:
    print("The student3 is added after append to the class_!")
```

这段代码打印”The student3 is added after append to the class\_!“

