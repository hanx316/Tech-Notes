# 使用Mac自带的Apache和PHP

MacOS自带装上了Apache和PHP，可以直接在命令行执行查看

```bash
# 查看php版本
php -v

# 查看apache版本
apachectl -v
```

### Apache服务基本操作

```bash
# 启动
apachectl start

# 关闭
apachectl stop

# 重启
apachectl restart
```

直接执行启动命令，访问localhost会看到一个显示**it works**的页面，说明服务启动成功，如果启动提示需要root权限可以加上`sudo`

刚才的页面实际上是以本机为服务器，共享的网络文件夹为`/library/webserver/documents`，访问的是里面的index.html.en文件

打开`/etc/apache2/httpd.conf`，找到含有PHP的一行，**LoadModule php5_module libexec/apache2/libphp5.so**，取消掉注释

然后找到下面这段，修改为自己设定的站点文件夹

```bash
DocumentRoot "/Library/WebServer/Documents"
<Directory "/Library/WebServer/Documents">

# 修改为
DocumentRoot "/Users/<username>/Sites"
<Directory "/Users/<username>/Sites">
```

其实就是mac的～目录下新建sites目录

再找到**Options FollowSymLinks Multiviews**，修改为**Options Indexes FollowSymLinks Multiviews**Mac10.10+的Apache需要修改这一行

修改时可能有权限问题，通过`sudo`输入密码即可

此时在我们设定的站点文件夹下新建一个index.html文件，然后开启apache服务，访问localhost就可以看到页面了

### 开启PHP配置

在`/etc`目录下有`php.ini.default`文件，直接复制一份为`php.ini`

然后修改刚才的index.html为index.php，随便写一点php代码，重启apache服务后再访问localhost应该能看到index.php返回的内容

输出phpinfo

```php
<?php phpinfo(); ?>
```


