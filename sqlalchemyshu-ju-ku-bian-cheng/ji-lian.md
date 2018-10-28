级联是在一对多关系中父表与子表进行联动操作的数据库术语。因为父表与子表通过外键关联，所以对父表或子表的增、删、改操作会对另一张表产生相应的影响。适当的利用级联可以开发出更优雅、健壮的数据库程序。本节学习SQLAlchemy中级联的操作方法。

> 注意：SQLAlchemy级联独立于SQL本身针对外键的级联定义。即使在数据库表定义中没有定义on delete等属性，也不影响开发者在SQLAlchemy中使用级联。

# 

# 

# 3、delete级联

顾名思义，delete级联是指当父对象被从session中删除时，其关联的子对象也自动被从session中delete。通过一个例子演示delete的作用，假设数据库中class表和students表的内容如下表所示：

class表：

| class\_id | name | level | address |
| :--- | :--- | :--- | :--- |
| 1 | 三年二班 | 3 | 理想路520号1楼 |
| 2 | 五年一班 | 5 | 理想路520号3楼 |
| 3 | 五年二班 | 5 | 理想路520号3楼 |

student表：

| student\_id | class\_id | name | age | gender | address | contactor |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 理想 | 10 | 男 | 静安区 | Null |
| 2 | 1 | mark | 10 | 女 | 静安区 | Null |
| 3 | 1 | 小马哥 | 9 | 女 | 闸口区 | 张三 |
| 4 | 2 | 张苗 | 10 | 男 | 宝山区 | NULL |
| 5 | 2 | 小黑 | 12 | 女 | 静安区 | 李四 |
| 6 | 2 | 喵喵 | 11 | 男 | 闸北区 | NULL |
| 7 | 1 | 韩永跃 | 10 | 男 | 静安区 | NULL |
| 8 | 3 | 小镜镜 | 12 | 男 | 闸北区 | NULL |
| 9 | 3 | 小镜子 | 12 | 女 | 宝山区 | NULL |

从例表中可知，系统中有3个班级，他们分别有4、3、2个学生。如果SQLAlchemy中没有把它们的relationship的cascade设置为delete，则删除父表内容不会删除相应的子表内容，而是把子表的相应外键设置为空。比如：

```
class_=session.query(Class).filter(name="三年二班").first() #三年二班的class_id为1
session.delete(class_)  #删除class_id为1的班级
```

当cascade不包含delete时，上述代码中的delete语句相当于执行了如下SQL语句：

```
UPDATE student SET class_id=None WHERE class_id=1;
DELETE FROM class WHERE class_id=1;
COMMIT;
```

执行后数据表class和student的内容变化如下所示：

class表：

| class\_id | name | level | address |
| :--- | :--- | :--- | :--- |
| 2 | 五年一班 | 5 | 理想路520号3楼 |
| 3 | 五年二班 | 5 | 理想路520号3楼 |

student表：

| student\_id | class\_id | name | age | gender | address | contactor |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 理想 | 10 | 男 | 静安区 | Null |
| 2 | 1 | mark | 10 | 女 | 静安区 | Null |
| 3 | 1 | 小马哥 | 9 | 女 | 闸口区 | 张三 |
| 4 | 2 | 张苗 | 10 | 男 | 宝山区 | NULL |
| 5 | 2 | 小黑 | 12 | 女 | 静安区 | 李四 |
| 6 | 2 | 喵喵 | 11 | 男 | 闸北区 | NULL |
| 7 | 1 | 韩永跃 | 10 | 男 | 静安区 | NULL |
| 8 | 3 | 小镜镜 | 12 | 男 | 闸北区 | NULL |
| 9 | 3 | 小镜子 | 12 | 女 | 宝山区 | NULL |

此时将表定义中的relationship的cascade属性设置为deleye

