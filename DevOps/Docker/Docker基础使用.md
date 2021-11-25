# Docker基础使用

一些常用的 docker 终端操作命令笔记。

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
docker pull [REPOSITORY]

# 拉取指定 tag 的镜像
docker pull [REPOSITORY[:TAG]]

# 列出所有本地镜像
docker images
docker image ls

# 列出所有本地镜像，包括隐藏镜像
docker images -a
docker image ls -a

# 列出所有容器id
docker images -q
docker image ls -q

# 查看某类镜像信息
docker images [REPOSITORY[:TAG]]

# 删除镜像
docker rmi <image-id> <image-id> ...
docker image rm <image-id> <image-id>

# 删除所有镜像
docker rmi $(docker images -q)
docker image rm $(docker images -q)

# 使用镜像新建容器并启动
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

# 使用 node 镜像执行命令，更多使用示例可以参考 Docker-MongoDB 实践
docker run node:8.11.4-alpine node -v
docker run node:8.11.4-alpine /bin/echo 'Hello world'

# 查看当前运行的容器
docker ps
docker container ls

# 查看所有容器
docker ps -a
docker container ls -a

# 查看最后 x 个容器，不论是否运行
docker ps -n x

# 停止运行的容器，可以通过容器 id 访问到容器
docker stop [OPTIONS] CONTAINER [CONTAINER...]
docker container stop CONTAINER

# 启动停止的容器
docker start [OPTIONS] CONTAINER [CONTAINER...]
docker container start CONTAINER

# 如果容器以后台方式运行或者运行时设置了交互模式后停止运行而重新启动时通常不会进入容器终端
# 这时可以用 attach 进入打开的容器，但是通过这种方式进入打开的容器，退出时会同时停止运行容器
docker attach CONTAINER

# 另一种方式是以在容器内新开进程的方式进入容器，其实就是在开启的容器内另外执行一些任务，例如：
docker exec -ti ubuntu_container /bin/bash

# 删除所有停止运行的容器
docker rm $(docker ps -a -q)
docker container prune
docker container prune -f

# 删除单个停止运行的容器
docker rm CONTAINER
docker container rm CONTAINER

# 强制删除所有容器，不论是否运行
docker rm -f $(docker ps -a -q)

# 自动重启容器(以 非0 代码退出时自动尝试 5 次重启)
docker run --restart=on-failure:5 IMAGE COMMAND
```

## 查看容器的运行日志

```bash
# 列出到目前容器的日志
docker logs CONTAINER

# 列出容器的最近 10 条日志
docker logs --tail 10 CONTAINER

# 持续输出容器的日志(这会输出容器的所有日志然后开始跟踪)
docker logs -f CONTAINER

# 持续输出容器最近的一条日志，并附带上时间戳
docker logs --tail 0 -ft test1
```

可以在 ubuntu 镜像上使用一个每秒循环输出 hello world 的小例子来尝试一下：

`docker run -d --name test ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"`

## 查看容器状态

```bash
# 查看容器基本信息(包括状态，创建时的设置，启动配置等非常全面的信息)
docker inspect CONTAINER

# 查看容器内运行的进程
docker top CONTAINER

# 查看容器统计信息(持续监听输出)
docker stats CONTAINER
```
