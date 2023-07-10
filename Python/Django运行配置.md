# Django运行配置
> Nginx、Django、uWSGI 搭建 Python Web 服务
---
## Nginx配置  

用nginx给uWSGI做反代，同时也能解决负载均衡、静态资源请求、加密证书等问题  

```nginx
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
---
## uWSGI配置
- 创建配置文件  
*/serve/uwsgi/uWSGI.ini*
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
---
## uWSGI服务
- 创建服务  
*/etc/systemd/system/uWSGI.service*
```ini
[Unit]
Description=uWSGI Service

[Service]
ExecStart=[/root/.pyenv/versions/web/bin]/uwsgi --ini /serve/uwsgi/uWSGI.ini
Restart=always

[Install]
WantedBy=multi-user.target
```
- 启动服务  
*uWSGI.service*
```bash
systemctl daemon-reload
systemctl enable uWSGI
systemctl start uWSGI
```

---
## Django配置
- settings.py
```python
# 将apps添加进环境变量
# 可将所有app放入其中,方便管理
import os,sys
sys.path.insert(0, os.path.join(BASE_DIR, 'apps'))

# Debug 仅建议开发环境开启
DEBUG = True
# Hosts 生产环境修改成相应域名
ALLOWED_HOSTS = ['*']

# 添加指定模板目录
TEMPLATES = [
    {
        # ...
        'DIRS': ['/www/wwwroot/example.com/templates'],
        # 是否在对应app/templates中查找模板
        'APP_DIRS': True,
        # ...
    },
]

# 语言
LANGUAGE_CODE = 'zh-hans'
# 时区
TIME_ZONE = 'Asia/Shanghai'

# 数据库
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'database',
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'USER': 'usrename',
        'PASSWORD': 'password',
    }
}
```
- 可能会遇到的问题
  - wsgi.py文件报错：找不到django模块解决方案  
```py
#在wsgi.py中更改，添加Django的包路径至系统Path变量

import sys
# 路径替换成Django包的位置
sys.path.append('/path/python3.11/site-packages')
```
