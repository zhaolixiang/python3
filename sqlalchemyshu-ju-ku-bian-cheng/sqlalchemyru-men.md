本节通过一套例子分析SQLAlchemy的使用方法。

使用SQLAlchemy至少需要3部分代码，它们分别是定义表、定义数据库连接、进行增、删、改、查等逻辑操作。

# 实例：

```
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column,Integer,String

Base=declarative_base()

class Accout(Base):
    __tablename__=u'accout'

    id=Column(Integer,primary_key=True)
    user_namr=Column(String(50),nullable=False)
    password=Column(String(200),nullable=False)
    title=Column(String(50))
    salary=Column(Integer)

    def is_active(self):
        #假设所有
        return True

    def get_id(self):
        #返回账号ID，用方法返回属性值提高了表的封装性。
        return self.id

    def is_authenticated(self):
        #假设已经通过验证
        return True

    def is_anonymous(self):
        #具有登陆名和密码的账号不是匿名用户
        return False
```

解析定义表的代码如下：

* SQLAlchemy表之前必须必须引入sqlalchemy.ext.declarative\_base，并定义一个它的实例Base。所有表必须继承自Base。本例中定义了一个账户表类Account。
* 通过\_\__tablename_\_\_属性定义了表在数据库中实际的名称account。
* 引入sqlalchemy包中的Column、Integer、String类型，因为需要用它们定义表中的列。本例在Account表中定义了5个列，分别是整型id和salary，以及字符串类型的user\_name、password、title。
* 在定义列时可以通过给Column传送参数定义约束。本例中通过primary_key参数将id列定义主键，通过nullable参数将user_\_name和password定义非空。
* 在表中还可以自定义其他函数。本例中定义了用户验证时常用的几个函数：is\__activite\(\)、get\__id\(\)、is_\_authenticate\(\)和is\_anonymous\(\)._



