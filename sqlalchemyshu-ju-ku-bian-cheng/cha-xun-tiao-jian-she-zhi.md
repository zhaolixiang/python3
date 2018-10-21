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

```

