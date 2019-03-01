man安装nginx

```
brew install nginx
nginx
```

此时就可以访问网址：

```
http://localhost:8080
```

准备编辑nginx的配置文件：

```
vim /usr/local/etc/nginx/nginx.conf
```

重启生效

```
sudo nginx -s reload
```



