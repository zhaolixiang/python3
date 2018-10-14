# 1、后台运行

一般调试过程中我们使用python app.py运行网站，方便我们在命令行中看运行状况。

但在生产环境下我们需要后台运行网站。我们可以使用linux的nohup命令。

```
nohup python app.py >> log/app.log &
```

# 2、supervisor守护进程

使用nuhup可以后台运行一个进程，但是一旦网站出现错误，进程关闭，网站将会停止运行。这时候就需要supervisor来帮我们守护进程，自动重启网站。

可在supervisor配置文件中添加：

```
[program:tornado-8000]
command=python /var/www/main.py --port=8000
directory=/var/www
user=www-data
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/tornado.log
loglevel=info
```

# 3、nginx代理多进程

虽然Tornado的内置IOLoop服务器可以直接作为运营服务器运行，但部署一个应用到生产环境面临着最大化利用系统资源的新挑战。因为Tornado架构的异步特性，无法用大多数Python网络框架标准WSGI进行站点部署，为了强化Tornado应用的请求吞吐量，在运营环境中通常采用反向代理+多Tornado后台实例的部署策略。

反向代理是代理服务器的一张。它根据客户端的请求，从后端的服务器上获取资源，然后将这些资源返回给客户端。当前最常用的开源反向代理服务器是Nginx：网站通过Internet DNS服务器将用户浏览器的访问定位到多台Nginx服务器上，每台Nginx服务器又将访问重定向到多台Tornado服务器上。多个Tornado服务器既可以部署在一台物理机上，也可以部署在多台物理机上。以资源最大化利用为目的，应该以每个物理机的CPU数量来决定分配在该台物理机上运行的Tornado实例数。

Nginx配置反向代理的方法非常简单，打开Nginx配置文件nginx.conf，进行类似如下配置，然后重启Nginx服务器即可：

```
user nginx;
worker_process 5;

error_log /var/log/nginx/error.log

pid /var/run/nginx.pid;

events{
use epoll;
}

proxy_next_upstream error;

upstream backs{
//配置3个后台Tornado服务
server 192.168.0.1:8001;
server 192.169.0.1:8002;
server 192.168.0.2:8003;
}

server{
listen 80; //监听80端口
server_name www.mysite.com;
}

location / {
proxy_pass http://backs;
}
```

除了一些标准配置，这个配置文件最重要的部分是upstram、listen和prox_pass指令。upstream backs{}定义了3个人后台Tornado服务的IP地址及各自的端口号；server{}中的listen定义了Nginx监听端口号80；proxy\_pass定义了所有对根目录的访问由之前定义的upstream backs中的服务器组提供服务，在默认情况下Nginx以循环方式分配到达的访问请求。_

# 4、nginx配合supervisor实现多进程

**在app.py中添加接受命令行参数：**

```
import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web

from tornado.options import define, options
define("port", default=8000, help="run on the given port", type=int)

class IndexHandler(tornado.web.RequestHandler):
    def get(self):
        greeting = self.get_argument('greeting', 'Hello')
        self.write(greeting + ', friendly user!')

if __name__ == "__main__":
    tornado.options.parse_command_line()
    app = tornado.web.Application(handlers=[(r"/", IndexHandler)])
    http_server = tornado.httpserver.HTTPServer(app)
    http_server.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()
```

**可在supervisor配置文件中添加：**

```
[program:tornado-8000]
command=python /var/www/main.py --port=8000
directory=/var/www
user=www-data
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/tornado.log
loglevel=info

[program:tornado-8001]
command=python /var/www/main.py --port=8001
directory=/var/www
user=www-data
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/tornado.log
loglevel=info
```

**使用nginx代理：**

```
user nginx;
worker_processes 5;
error_log /var/log/nginx/error.log;
pid /var/run/nginx.pid;
events {
    worker_connections 1024;
    use epoll;
}
proxy_next_upstream error;
upstream tornadoes {
    server 127.0.0.1:8000;
    server 127.0.0.1:8001;
    server 127.0.0.1:8002;
    server 127.0.0.1:8003;
}
server {
    listen 80;
    server_name www.example.org *.example.org;
    location /static/ {
        root /var/www/static;
        if ($query_string) {
            expires max;
        }
    }
    location / {
        proxy_pass_header Server;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_pass http://tornadoes;
    }
}
```



