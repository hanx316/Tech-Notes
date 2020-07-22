# Docker-MySQL 实践

熟悉 Docker 之后大抵是不太想在自己机器上装这些开发依赖软件了，通过镜像和容器的方式管理起来心里也更有数（安装，更新，删除）。

流程基本上可以参考 Docker 使用 MongoDB 的思路(另一篇 Docker-MongoDB 实践)，只是在命令细节上有所不同。

## 下载镜像

版本选择当前最新的 MySQL 8，具体信息和使用方式可以查看 [MySQL 的 Docker 官方镜像地址](https://hub.docker.com/_/mysql)

```bash
docker pull mysql:8
```

## 开启 MySQL 服务

MySQL 和 MongoDB 不同的是在开启容器时需要指定 root 用户的密码，每次启动时也需要指定用户和密码，其余容器端口映射和文件挂载都是相同的思路。

MySQL 的默认使用端口是 3306。

关于文件挂载，其实对于数据库哪些文件需要挂载这方面需要熟悉数据库的文件目录结构以及实际的需求，自己机器上开发使用的话，只要不去删除容器其实挂载与否都无所谓了。

```bash
# 确定一个挂在容器 MySQL 文件的主机目录
pwd
mkdir mysql && cd mysql
mkdir db

# 启动容器
docker run -d --name mysql8 -p 3306:3306 -v $PWD/db:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=<your_root_password> mysql:8

# 以 root 用户进入 MySQL shell
docker exec -it mysql8 sh -c 'exec mysql -uroot -p"<your_root_password>"'
```

启动容器之后，各种 MySQL 可视化软件也可以通过连接 `localhost:3306` 到本地数据库了。
