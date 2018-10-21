SQLAlchemy这样的orm数据库操作方式可以对业务开发者屏蔽不同数据库之间的差异，这样当需要进行数据库迁移时（比如MySQL迁移到SQLite），则只需要更换数据库连接字符串。

下表列出了SQLAlchemy连接主流数据库时的数据库字符串的编写方法：

| 数据库 | 连接字符串 |
| :--- | :--- |
| Microsoft SQLServer | 'mssql+pymssql://\[user\]:\[pass\]@\[domain\]:\[port\]/\[dbname\]' |
| MySQL | 'mysql://\[user\]:\[pass\]@\[domain\]:\[port\]/\[dbname\]' |
| Oracle | 'oracle://\[user\]:\[pass\]@\[domain\]:\[port/\[dbname\]\]' |
| PostgreSQL | 'postgresql://\[user\]:\[pass\]@\[domain\]:\[port\]/\[dbname\]' |
| SQLite | 'sqlite://\[file\_pathname\]' |



