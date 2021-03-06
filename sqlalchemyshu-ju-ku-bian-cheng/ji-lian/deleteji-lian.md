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
| 1 | NULL | 理想 | 10 | 男 | 静安区 | Null |
| 2 | NULL | mark | 10 | 女 | 静安区 | Null |
| 3 | NULL | 小马哥 | 9 | 女 | 闸口区 | 张三 |
| 4 | 2 | 张苗 | 10 | 男 | 宝山区 | NULL |
| 5 | 2 | 小黑 | 12 | 女 | 静安区 | 李四 |
| 6 | 2 | 喵喵 | 11 | 男 | 闸北区 | NULL |
| 7 | 1 | 韩永跃 | 10 | 男 | 静安区 | NULL |
| 8 | 3 | 小镜镜 | 12 | 男 | 闸北区 | NULL |
| 9 | 3 | 小镜子 | 12 | 女 | 宝山区 | NULL |

此时将表定义中的relationship的cascade属性设置为delete:

```
students=relationship("Student",backref="class",cascade="delete")
```

现在通过如下语句删除“五年一班”：

```
class_=session.query(Class).filter(name="五年一班").first()  #五年一班的class_id为2
session.delete(class_)          #删除class_id为2的班级
```

当cascade包含“delete”时，上述代码中的delete语句相当于执行了如下SQL语句：

```
DELETE FROM student WHERE class=2;
DELETE FROM class WHERE class=2;
COMMIT;
```

执行后数据库表class和student的内容变化如下表所示：

class表：

| class\_id | name | level | address |
| :--- | :--- | :--- | :--- |
| 3 | 五年二班 | 5 | 理想路520号3楼 |

student表：

| student\_id | class\_id | name | age | gender | address | contactor |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | NULL | 理想 | 10 | 男 | 静安区 | Null |
| 2 | NULL | mark | 10 | 女 | 静安区 | Null |
| 3 | NULL | 小马哥 | 9 | 女 | 闸口区 | 张三 |
| 7 | NULL | 韩永跃 | 10 | 男 | 静安区 | NULL |
| 8 | 3 | 小镜镜 | 12 | 男 | 闸北区 | NULL |
| 9 | 3 | 小镜子 | 12 | 女 | 宝山区 | NULL |



