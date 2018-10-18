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



