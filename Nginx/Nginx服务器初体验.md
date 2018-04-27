# Nginx服务器初体验

最近使用Nginx在自己的服务器部署了一个静态网站，没有做任何服务器上的优化，流程非常简单，还是记一下

首先是安装Nginx，我是centos系统，新手就先直接采用平台的默认包安装了，以后玩熟了可以再研究源码编译安装

```bash
yum install nginx -y
```

安装好以后直接启动，就开启了服务，默认是80端口，这时直接在公网用http协议访问自己ip能看到Nginx的默认页面

```bash
nginx
```

接下来修改配置，自定义web主机目录

Nginx的配置文件在`/etc/nginx/nginx.conf`，打开找到`root /usr/share/nginx/html`这一行，这个就是Nginx的默认web目录，按照官方文档，可以修改为`root /data/www`，当然也可以改成其他自己的路径，方便维护即可

然后重启服务，让新配置生效

```bash
nginx -s reload
```

下来在自己设定的web目录下放好对应的html文件，就可以在浏览器访问了

一些常用命令，Nginx通过信号来控制进程，-s参数就是signal传递信号的意思

```bash
# 启动服务
nginx

# 重新载入配置
nginx -s reload

# 重启服务
nginx -s reopen

# 关闭服务
nginx -s stop

# 查看版本号
nginx -v

# 查看版本号及编译器版本和配置信息
nginx -V

# 查看进程相关信息
ps -ef|grep nginx
```

另，附上一个不错的Nginx学习参考网站[nginx中文文档](http://www.nginx.cn/doc/)