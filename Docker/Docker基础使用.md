# Docker基础使用

## 帮助命令查询

Docker 命令提供了非常不错的帮助信息，方便查询学习和使用。

```bash
# 查询 docker 命令的帮助信息
docker --help

# 查询 docker 二级命令的帮助信息
docker COMMAND --help
# e.g
docker images --help
```

## 镜像和容器操作常用命令和一些示例

```bash
# 搜索关键词相关镜像
docker search [OPTIONS] TERM

# 搜索信息过滤，e.g: 搜索 star 30以上的 ubuntu 镜像
docker search -f=stars=30 ubuntu

# 拉取镜像，默认拉取 tag 为 latest 的镜像
docker pull <name>

# 拉取指定 tag 的镜像
docker pull <name>:<tag>

# 列出所有本地镜像
docker images
docker image ls

# 列出所有本地镜像，包括隐藏镜像
docker images -a
docker image ls -a

# 列出所有容器id
docker images -q
docker image ls -q

# 删除镜像
docker rmi <image-id> <image-id> ...
docker image rm <image-id> <image-id>

# 删除所有镜像
docker rmi $(docker images -q)
docker image rm $(docker images -q)

# 使用镜像新建容器并启动
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

# 使用 node 镜像执行命令，更多使用示例可以参考 Docker-MongoDB实战
docker run node:8.11.4-alpine node -v
docker run node:8.11.4-alpine /bin/echo 'Hello world'

# 查看当前运行的容器
docker ps
docker container ls

# 查看所有容器
docker ps -a
docker container ls -a

# 停止运行的容器，可以通过容器 id 访问到容器
docker stop [OPTIONS] CONTAINER [CONTAINER...]
docker container stop CONTAINER

# 启动停止的容器
docker start [OPTIONS] CONTAINER [CONTAINER...]
docker container start CONTAINER

# 删除所有停止运行的容器
docker container prune
docker container prune -f

# 删除单个停止运行的容器
docker container rm CONTAINER

```
