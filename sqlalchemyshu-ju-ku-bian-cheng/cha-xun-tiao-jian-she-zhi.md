在实际编程中需要根据各种不同的条件查询数据库记录，SQLAlchemy查询条件被称为过滤器。这里列出了最常用的过滤器的使用方法。

# 1、等值过滤器（==）

等值过滤器用于判断某列是否等于某值，是最常用的过滤器。

```
session.query(Account).filter(Account.user_name=='Mark') #判断字符串类型
session.query(Account).filter(Account.salary==2000) #判断数值类型
```

# 2、不等过滤器（!=、&lt;、&gt;、&lt;=、&gt;=）

与等值过滤器相对的是不等于过滤器，不等于过滤器可以延伸为几种形式：不等于、小于、大于、小于等于、大于等于。

```
session.query(Account).filter(Account.user_name !="mark" ) #不等于字符串类型
session.query(Account).filter(Account.salary !=2000)       #不等于数值类型
session.query(Account).filter(Account.salary >2000)       #大于过滤器
session.query(Account).filter(Account.salary <2000)       #小于过滤器
session.query(Account).filter(Account.salary <=2000)       #小于等于过滤器
session.query(Account).filter(Account.salary >=2000)       #大于等于过滤器
```

# 3、模糊查询（like）

模糊查询适用于只知道被查询字符串的一部分内容时，通过设置通配符的位置，可以查询出不同的结果。通配符用百分号%表示。

假设表中的数据为：

| id | user\_name | title | salary |
| :--- | :--- | :--- | :--- |
| 1 | David Li | System Manager | 3000 |
| 2 | Debeca Li | Accountant | 3000 |
| 3 | David Backer | Engineer | 3000 |
| 4 | Siemon Bond | Enfineer | 4000 |
| 5 | Van Berg | General Manager | NULL |

```
#查询所有名字包含字母i的用户，结果包括id为1、2、3、4的4条记录
session.query(Account).filter(Account.user_name.like('%i%'))

#查询所有title中以Manager结尾的用户，结果包括id为1、5的两条记录
session.query(Account).filter(Account.title.like('%Manager'))

#查询所有名字中以Da开头的用户，结果包括id为1、3的两条记录
session.query(Account).filter(Account.user_name.like('Da%'))
```

> 注意：模糊查询只适用于查询字符串类型，不适用于数值类型。

# 4、包括过滤器（in\_）

当确切的知道要查询记录的字段内容，但是一个字段有多个内容要查询时，可以用包含过滤器。

```
#查询id不为1，3，5的记录，结果包含id为2，4的两条记录
session.query(Account).filter(~Account.id.in_([1,3,5]))
#查询工资不为2000、3000、4000的记录，结果包含id为5的1条记录
session.query(Account).filter(~Account.id.in_([2000,3000,4000]))
#查询所有title不为Engineer和Accountant记录，结果包括id为1、5的两条记录
session.query(Account).filter(~Account.title.in_(['Accountant','Engineer']))
```

# 5、判断是否为空（is NULL、is not NULL）

空值NULL是数据库字段中比较特殊的值。在SQLAlchemy中支持对字段是否为空进行判断。判断时可以用等值、不等值过滤器筛选，也可以用is、isnot进行筛选。

```
#查询salary为空值的记录，结果包含id为5的记录
#下面两方式效果相同
session.query(Account).filter(Account.salary==None)
session.query(Account).filter(Account,salary.is_(None))

#查询salary不为空值的记录，结果包含id为1、2、3、4的记录
#下面两方式效果相同
session.query(Account).filter(Account.salary!=None)
session.query(Account).filter(Account.salary.isnot(None))
```

# 6、非逻辑（~）

当需要查询不满足某条件的记录时可以使用非逻辑。

```
#查询id不为1、3、5的记录，结果包含id为2、4的两条记录
session.query(Account).filter(~Account.id.in_([1,3,5]))

#查询工资不为2000、3000、4000的记录，结果包含id为5的1条记录
session.query(Account).filter(~Account.id.in_([2000,3000,4000]))

#查询所有title不为Engineer和Accountant的记录，结果包括id为1、5的2条记录。
session.query(Account).filter(~Account.title.in(['Accountant','Engineer']))
```

# 7、与逻辑（and\_）

当需要查询同时满足多个条件的记录时，需要用到与逻辑。在SQLAlchemy中与逻辑可以有3种表达方式。

以下3条语句查询结果相同，都是id为3的记录。

```
#直接在filter中添加多个条件即表示与逻辑
session.query(Account).filter(Account.title=='Engineer',Account.salary=3000)

#用关机子and_进行逻辑查询
from sqlalchemy import and_
session.query(Account).filter(and_(Account.title=='Engineer',Account.salary=3000))

#通过多个filter的链接表示与逻辑
session.query(Account).filter(Account.title=='Engineer').filter(Account.salary=3000)
```

# 8、或逻辑（or\_）

当需要查询多个条件但只需其中一个条件满足时，需要用到或逻辑。

```
#引入或逻辑关键字or_
from sqlalchemy import or_

#查询title是Engineer或者salary为3000的记录，返回
```



