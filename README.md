# Tech-Notes

📒学习技术的笔记，好记性不如 Markdown

#### 查询备忘备用：

- [Github Markdown 文档](https://help.github.com/categories/writing-on-github/)

- SS Server 一键安装脚本1：
```
wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

- SS Server 一键安装脚本2：
```
yum -y install wget

wget -N –no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```

#### 关注的 js 模块：

- [components](https://github.com/component) 看到 TJ 在成员里面，大概就是国外大神们搞的一个模块集

- [node-modules](https://github.com/node-modules) 阿里自用的 node 模块集
