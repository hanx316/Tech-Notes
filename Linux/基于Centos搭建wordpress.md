# 基于Centos搭建wordpress

本文主要基于腾讯云的实验，做一下笔记，以便备忘。

### 安装Nginx

```bash
yum install nginx -y
```

去除对IPv6地址的监听

```bash
vi /etc/nginx/conf.d/default.conf
```

注释这一句`listen [::]:80 default_server;`

然后启动nginx

```bash
nginx

# 设置nginx开机启动
chkconfig nginx on
```

### 安装MySQL

```bash
# 安装mysql
yum install mysql-server -y

# 启动服务
service mysqld start

# 设置mysql密码
/usr/bin/mysqladmin -u root password 'mypassword'

# 设置mysql开机启动
chkconfig mysqld on
```

### 安装PHP

