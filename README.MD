# Walle 安装指南

-源文档地址：[https://www.walle-web.io/docs/installation.html](https://www.walle-web.io/docs/installation.html)

## 依赖Ubuntu 18.4(bonic)
### 1. 国内切换apt镜像地址
```
cd /etc/apt
```
```
# mv sources.list sources.list.bak
vim sources.list
```
```
apt-get update
```
```
apt-get install gcc
```
```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```
### 继续执行脚本
```
sh admin.sh init
```
```
virtualenv --no-site-packages -p /usr/bin/python2.7 venv
```
### nginx配置
- 将下列代码复制到/etc/nginx/nginx.conf（http下）
```
upstream webservers {
    server 0.0.0.0:5000 weight=1; # 负载设置
}

server {
    listen       8000; # 默认80貌似不行
    server_name  admin.walle-web.io; # 域名设置
    access_log   /usr/local/nginx/logs/walle.log main;
    index index.html index.htm; # 日志目录

    location / {
        try_files $uri $uri/ /index.html;
        add_header access-control-allow-origin *;
        root /walle-web/fe; # 前端代码已集成到walle-web，即walle-web/fe的绝对路径
    }

    location ^~ /api/ {
        add_header access-control-allow-origin *;
        proxy_pass      http://webservers;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header    Origin        $host:$server_port;
        proxy_set_header    Referer       $host:$server_port;
    }

    location ^~ /socket.io/ {
        add_header access-control-allow-origin *;
        proxy_pass      http://webservers;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header    Origin        $host:$server_port;
        proxy_set_header    Referer       $host:$server_port;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;

        # WebScoket Support
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```
### 数据库准备
```
cd /path/to/walle-web/walle/config
```
```
vim settings_prod.py
```
- 修改需要配置的地方，如数据库的用户名密码等
### 返回并继续
```
mysql  -hlocalhost -uroot -p -e'CREATE SCHEMA walle'
```
- h后跟host地址
- u后跟用户名
### 数据库初始化
```
bash admin.sh migration
```
### 启动
```
bash admin.sh start (bash admin.sh restart)
```
