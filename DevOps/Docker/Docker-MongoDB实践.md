# Docker-MongoDB 实践

Docker 安装 MongoDB 环境的实践笔记。

## 下载镜像

这里使用比较常用的 mongo3.4 版本，具体信息和使用方式可以查看 [mongo 的 Docker 官方镜像地址](https://hub.docker.com/_/mongo/)

```bash
docker pull mongo:3.4
```

## 开启 mongo 服务

其实就是运行镜像的默认命令开启容器，但是需要注意映射容器的端口到主机，另外为了方便数据存储，需要在主机确定 `db` 的目录并挂载到容器的 `/data/db` 目录。

```bash
# 确定主机存储数据的目录
pwd
mkdir mongodb && cd mongodb
mkdir db

# 启动容器，将当前目录下的 /db 挂载到容器 /data/db
docker run -d --name mongo3.4 -p 27017:27017 -v $PWD/db:/data/db mongo:3.4

# 进入 mongo shell 其中 mongo 是打开 mongo shell 的命令
docker exec -it mongo3.4 mongo

# 也可以通过设置 host 参数为主机 ip 进入 mongo shell，但是会新开启一个容器
docker run -it mongo:3.4 mongo --host [主机 ip]
```

启动 mongo 服务容器之后，Robo 3T 之类的可视化工具就可以通过连接 `localhost` 连接数据库了。

## 导入数据到容器

首先将需要导入的数据文件放置在挂载的目录 `$PWD/db` 下以便容器能够访问，导入成功后可以删除

```bash
# 启动容器的 bash 终端
docker exec -it mongo3.4 bash

# 通过 mongoimport 导入
mongoimport --db DB --collection COLLECTION --drop --file /data/db/dataset.json
```

还可以通过 mongorestore 导入，主要针对 mongodump 备份的数据（通常是 `/dump/...` 这样一个文件目录）。

将 dump 文件置于 `/data` 下，可以先放在容器挂载的宿主目录下，然后进容器移动文件，事后删掉容器即可。

```bash
# 进入容器后
cd /data
# 检查是否有 dump 文件
ls | grep dump
# go..
mongorestore
```
