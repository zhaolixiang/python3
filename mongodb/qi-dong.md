```
service mongodb start
```

运行外部访问：

```
vim /etc/mongod.conf

# /etc/mongod.conf

# 监听本地和局域网接口。
bind_ip = 127.0.0.1,192.168.161.100
```

测试：

```
post={"title":"MyTitle","content":"MyContent","date":new Date()}
```



