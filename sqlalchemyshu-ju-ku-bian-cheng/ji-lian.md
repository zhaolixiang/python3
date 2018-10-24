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



