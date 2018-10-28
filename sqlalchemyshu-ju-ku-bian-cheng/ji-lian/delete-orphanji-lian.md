# 4、delete-orphan级联

delete-orphan级联是指当子对象不再与任何父对象关联时，会自动将该子对象删除。设置父表与子表的relationship中的cascade包含“delete-orphan”：

```
students=relationship("Student",backref="class",cascade="delete-orphan")
```

通过如下代码将于班级“五年一班”关联的学生全部脱离：

```
class_=session.query(Class).filter(name="五年二班").first()
unattachedStudent=[]
while len(class_.students)>0:
    unattachedStudent.append(class_.students.pop()) #与父对象脱离
session.commit    #显示地提交事务
```

上述代码中没有显示地删除任何学生，但由于使用了delete-orphan级联，所以被脱离出班级对象的学生会在session事务提交时会自动从数据库中删除。代码执行后数据库表中的内容的变化：

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



