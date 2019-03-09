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

配置

```
server {
        listen       8080;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        location / {
    rewrite  ^.+apis/?(.*)$ /$1 break;
    include  uwsgi_params;
       proxy_pass   http://localhost:8000;
       }
       location /api{
       proxy_pass http://localhost:5001;
       }


     }
```

重启生效

```
sudo nginx -s reload
```



