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



