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

直接执行启动命令，访问localhost会看到一个显示**it works**的页面，说明服务启动成功

