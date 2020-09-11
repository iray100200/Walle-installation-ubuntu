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
sh admin.sh migration
```
