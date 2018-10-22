关系数据库是建立在关系模型基础上的数据库，所以表之间的关系在数据库编程中尤为重要。本节围绕在SQLAlchemy中如何定义关系及如何使用关系进行查询进行讲解，使读者能够快速掌握SQLAlchemy的关系操作。

# 1、案例

设计3个实体表：班级表class、学生表student、老师表teacher和1个关系表：class\_teacher。班级与学生为一对多关系，班级与老师之间为多对多关系。

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

    class_teachers=relationship("ClassTeacher",backref="class")
    students=relationship("Student",backref="class")

class Student(Base):
    __tablename__='student'
    student_id=Column(Integer,primary_key=True)
    name=Column(String(50))
    age=Column(Integer)
    gender=Column(String(10))
    address=Column(String(50))
    class_id=Column(Integer,ForeignKey('class.id'))

class Teacher(Base):
    __tablename__='teacher'
    teacher_id=Column(Integer,primary_key=True)
    name=Column(String(50))
    gender=Column(String(10))
    telephone=Column(String(50))
    address=Column(String(50))
    class_teachers=relationship("ClassTeacher",backref="teacher")

class ClassTeacher(Base):
    __tablename__='class_teacher'
    teacher_id=Column(Integer,ForeignKey('teacher.teacher_id'),primary_key=True)
    class_id=Column(Integer,ForeignKey("class.id"),primary_key=True)
```



