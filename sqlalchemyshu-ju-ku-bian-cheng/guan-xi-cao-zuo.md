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

代码中用了4个SQLAlchemy模型对4个表进行了定义，其中与关系定义相关的部分如下：

* 外键设置：在列的定义中，为Column传入ForeignKey进行外键设置。

```
class_id=Column(Integer,ForeignKey('class.id'))
```

* 关系设置：通过relationship关键字在父模型中建立对字表的引用，例如Class模型中的关系设置如下：

```
students=relationship("Student",backref="calss")
```

其中的backref参数为可选参数，如果设置backref，则此语句同时设置了 从父表对子表的引用。

* 一对多关系的使用：以后可以直接通过该students属性获得相关班级中所有学生的信息。如下代码可以打印班级【三年二班】的所有学生信息。

```
class=session.query(Class).filter(Clss.name=="三年二班").first()

for student in class_.students:
     print(student)
```

* 多对多关系的使用：通过关联模型ClassTeacher实现，在其中分别设置模型Class和Teacher的外键，并且在父模型中设置相应的relationship实现。多对多关系也可以想象成一个关联表，分别对两个父表实现了多对一的关系。班级与老师之间为多对多的关系，如下代码可以打印班级【三年二班】中所有老师的信息

```
class=session.query(Class).filter(Class.name=="三年二班").first()
for class_teacher in class_.class_teachers:
     teacher=class_teacher.teacher
     print(teacher)
```

> 上述代码中class\_teacher.teacher是在模型teacher中针对ClassTeacher定义的反向引用。

# 2、连接查询

在实际开发中，有了关系就必不可少地会有多表连接查询的需求。下面通过实际例子演示如果进行多表连接查询。

在查询语句中可以使用join关键字进行连接查询，打印出所有三年级学生的姓名：

```
students=session.query(Student).join(Class).filter(Class.level==3).all()
for student in students:
    print(student.namr)
```

上述查询函数会自动把外键关系作为连接条件，该查询被SQLAlchemy自动翻译为如下SQL语句并执行：

```
SELECT student.student_id AS student_student_id,
       student.name AS student.name,
       student.age AS student.age,
       student.gender AS student.gender,
       student.address AS student.address,
       student.class_id AS student_class_id
FROM student JOIN class ON student.class_id=class.class_id
WHERE class.leve=?
(3,)
```

如果需要将被连接表的内心同样打印出来，则可以在query中指定多个表对象。

下面的语句在打印出所有三年级学生姓名的同时，打印出其所在班级的名字。

```
for student,class_ in session.query(Student,Class).join(Class).filter(Class.level==3).all():
    print(student.name,class_.name)
```

上述查询函数会自动把外键关系作为连接条件，该查询被SQLAlchemy自动翻译为如下SQL语句并执行：

```
SELECT student.student_id AS student_student_id,
       student.name AS student.name,
       student.age AS student.age,
       student.gender AS student.gender,
       student.address AS student.address,
       student.class_id AS student_class_id,
       class.class_id AS class_class_id,
       class.name AS class_name,
       class.level AS class_level,
       class.address AS class_location
FROM student JOIN class ON student.class_id=class.class_id
WHERE class.leve=?
(3,)
```

如果需要用除外键外的其他字段作为连接条件，则需要开发者在join中自行设置。下面打印出所有班级的address与学生的address相同的学生的姓名：

```

```

