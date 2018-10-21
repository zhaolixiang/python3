本节通过一套例子分析SQLAlchemy的使用方法。

使用SQLAlchemy至少需要3部分代码，它们分别是定义表、定义数据库连接、进行增、删、改、查等逻辑操作。

# 定义表的实例：

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
* 在表中还可以自定义其他函数。本例中定义了用户验证时常用的几个函数：is\__activite\(\)、get\_\_id\(\)、is\_\_authenticate\(\)和is\_anonymous\(\)。

# 定义数据库连接的示例代码如下：

```
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session,sessionmaker
from contextlib import contextmanager

db_connect_string='mysql://v_user:v_pase@localhost:3306/test_database?charset=utf8'

ssl_args={
    'ssl':{
        'cert':'/home/ssl/client-cert.pem',
        'key':'/home/shouse/ssl/client-key.pem',
        'ca':'/home/shouse/ssl/ca-cert.pem'
    }
}
engine=create_engine(db_connect_string,connect_args=ssl_args)
SessionType=scoped_session(sessionmaker(bind=engine,expire_on_commit=False))
def GetSession():
    return SessionType()

@contextmanager
def session_scope():
    session=GetSession()
    try:
        yield session
        session.commit()
    except:
        session.rollback()
        raise
    finally:
        session.close()
```

解析此连接数据部分的代码如下：

* 引入数据库和会话引擎：sqlalchemy.create\__engine、sqlalchemy.orm.scoped_\_session、sqlalchemy.orm.sessionmaker。
* 定义连接数据库需要用到的数据库字符串。本例连接MySQL数据库，字符串格式为\[databse\__type\]://\[user_\_name\]:\[password\]@\[domain\]:\[port\]/\[database\]?\[parameters\]。本例中除了必须的连接信息，还传入了charset参数，指定用utf-8编码方式解码数据库中的字符串。
* 用create\_engine建立数据库引擎，如果数据库开启了SSL链路，则在此处需要传入ssl客户端证书的文件路径。
* 用scoped\_session\(sessionmaker\(bind=engine\)\)建立会话类型SessionType，并定义函数GetSession\(\)用以创建SessionType的实例。

至此，已经可以用GetSession\(\)函数创建数据库会话并进行数据库操作了。但为了使之后的数据库操作的代码能够自动进行事务处理，本例中定义了上下文函数session\_scope\(\)。在Python中定义上下文函数的方法是为其加入contextlib包中的contextmanager装饰器。在上下文函数中执行如下逻辑：在函数开始时建立数据库会话，此时会自动建立一个数据库事务：当发生异常时回滚（rollback）事务；当退出时关闭（close）连接。在关闭连接时会自动进行事务提交（commit）操作。

# 进行数据库操作的代码：

```

```



