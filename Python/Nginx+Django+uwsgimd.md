# Nginx+Django+uwsgi
> ### (Python Web开发环境)
***
- ## Nginx配置
```ini
server {
        listen       80;
        server_name  localhost;
        
        location / {            
            include  uwsgi_params;
            uwsgi_pass  127.0.0.1:8001;
            uwsgi_param UWSGI_SCRIPT Mysite.wsgi;
            uwsgi_param UWSGI_CHDIR /Mysite;
            index  index.html index.htm;
            client_max_body_size 35m;
        }
    }
```
***
- ## uwsgi配置
```ini
[uwsgi]
#监视python模块自动重载
#生产环境建议关闭
python-autoreload = 1
#启动用户和组
uid = www
gid = www
#IP及端口设置
socket = 127.0.0.1:8001
#主进程
master = true
#多站模式
vhost = true
#无入口模式
no-site = true
#进程数
workers = 5
#平滑启动
reload-mercy = 10
#退出清理文件
vacuum = true
#单进程最大请求数
max-requests = 1000
#进程总内存上限
limit-as = 512
#缓冲字节
buffer-size = 65536
#请求超时丢弃
harakiri = 60
#请求超时丢弃生成日志
harakiri-verbose = true
#虚拟环境
#virtualenv = /www/wwwroot/uwsgi
#主进程pid文件
#pidfile = /var/run/uwsgi.pid
#日志
daemonize = /www/wwwlogs/uwsgi.log
#日志文件大小,字节
log-maxsize=1024000
```
*提示wsgi中找不到django模块,则手动将python虚拟环境的site-packages目录加入到环境变量中*
- eg : *将下方代码加入到wsgi入口引用django之前*
```py
import sys
sys.path.append('/path/python3.11/site-packages')
```
