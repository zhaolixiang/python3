级联是在一对多关系中父表与子表进行联动操作的数据库术语。因为父表与子表通过外键关联，所以对父表或子表的增、删、改操作会对另一张表产生相应的影响。适当的利用级联可以开发出更优雅、健壮的数据库程序。本节学习SQLAlchemy中级联的操作方法。

> 注意：SQLAlchemy级联独立于SQL本身针对外键的级联定义。即使在数据库表定义中没有定义on delete等属性，也不影响开发者在SQLAlchemy中使用级联。

# 1、级联定义

SQLAlchemy中的级联通过对父表中的relationship属性定义cascade参数来实现，代码如下：

```
from sqlalchemy import Table,Column,Integer,ForeignKey,String
from sqlalchemy.orm import relationship,backref
from sqlalchemy.ext.declarative import declarative_base

Base=declarative_base()

class Class(Base):
    __tablename__='class'
    class_id=Column(Integer,primary_key=True)
    name=Column(String(50))
    level=Column(Integer)
    address=Column(String(50))

    students=relationship("Student",backref="class_",cascade="all")

class Student(Base):
    __tablename__='student'
    student_id=Column(Integer,primary_key=True)
    name=Column(String(50))
    age=Column(Integer)
    gender=Column(String(10))
    address=Column(String(50))
    class_id=Column(Integer,ForeignKey('class.class_id'))
```

上述代码定义了班级表Class（父表）和学生表Student（子表）。一对多的关系有父表中的relationship属性students进行定义。relationship中的cascade参数定义了要在该关系上实现的级联方法为：all。

SQLAlchemy中另外一种设置级联的方式是在子表的relationship的backref中进行设置。比如上述代码可以写为如下形式，意义保持不变：

```
from sqlalchemy import Table,Column,Integer,ForeignKey,String
from sqlalchemy.orm import relationship,backref
from sqlalchemy.ext.declarative import declarative_base

Base=declarative_base()

class Class(Base):
    __tablename__='class'
    class_id=Column(Integer,primary_key=True)
    name=Column(String(50))
    level=Column(Integer)
    address=Column(String(50))
class Student(Base):
    __tablename__='student'
    student_id=Column(Integer,primary_key=True)
    name=Column(String(50))
    age=Column(Integer)
    gender=Column(String(10))
    address=Column(String(50))
    class_id=Column(Integer,ForeignKey('class.class_id'))
    class_=relationship("Class",backref="students",cascade="all")
```

上述代码没有在父表Class中设置relationship和cascade，而是在子表中设置了。

SQLAlchemy中可选的cascade取值范围如下表所示：

| 可选值 | 意义 |
| :--- | :--- |
| save-update | 当一个父对象被新增到session中时，该对象当时关联的子对象也自动被新增到session中。 |
| merge | Session.merge是一个对数据库对象进行新增或更新的办法。cascade取值为merge时的意义为：当父对象进行merge操作时，该对象当时关联的子对象也会被merge |
| expunge | Session.expunge是一种将对象从session中移除的方法。cascade取值为expunge时的意义为：当父对象进行了expunge操作时，该对象当时关联的子对象也会被从session中删除。 |
| delete | 当父对象被delete时，会自动将该子对象删除 |
| delete-orphan | 当子对象不再与任何父对象关联时，会自动将该子对象删除 |
| refresh-expire | Session.expire是一种设置对象已过期、下次引用时需要从数据库即时读取的方法。cascade取值为refredh-expire时的意义为：当父对象进行了expire操作时，该对象当时关联的子对象也进行expire操作。 |
| all | 是一个集合值，表示：save-update、merge、refresh-expire、expunge、delete同时被设置 |

多个cascade属性可以通过逗号分隔并同时赋值给cascade。例如：

```
students=relationship("Student",backref="class_",cascade="save-update,merge,expunge")
```

在默认情况下，任何relationship的级联属性都被设置为cascade="save-update,merge"。下面就常用的参数：save-update、delet、delete-orphan的功能进行举例说明。

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

# 3、delete级联

顾名思义，delete级联是指当父对象被从session中，



