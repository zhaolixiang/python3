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

