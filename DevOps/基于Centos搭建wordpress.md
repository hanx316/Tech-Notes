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

此时直接访问主机的公网ip可以看到nginx页面

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

```bash
# 安装PHP
yum install php-fpm php-mysql -y

# 启动php-fpm进程
service php-fpm start

# 查看php-fpm监听端口
netstat -nlpt | grep php-fpm

# 设置php-fpm开机启动
chkconfig php-fpm on
```

### 安装Wordpress

```bash
yum install wordpress -y
```

安装成功以后可以在`/usr/share/wordpress`下看到wordpress的源码

- 配置数据库

```bash
# 进入mysql
mysql -uroot --password='mypassword'

# 创建数据库
CREATE DATABASE wordpress;

# 退出mysql环境
exit
```

同步DB配置`wp-config.php`，主要是数据库name, username, password

- 配置Nginx，将请求转发给php-fpm来处理

```bash
# 重命名配置文件
cd /etc/nginx/conf.d/
mv default.conf default.conf.bak

# 创建wordpress.conf配置
vi wordpress.conf
```

内容如下

```
server {
  listen 80;
  root /usr/share/wordpress;
  location / {
    index index.php index.html index.htm;
    try_files $uri $url/ /index.php index.php;
  }
  # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
  location ~ .php$ {
    fastcgi_pass    127.0.0.1:9000;
    fastcgi_index   index.php;
    fastcgi_param   SCRIPT_FILENAME   $document_root$fastcgi_script_name;
    include         fastcgi_params;
  }
}
```

然后重新加载nginx`nginx -s reload`

这时候直接访问主机的公网ip应该就能看到wordpress了
