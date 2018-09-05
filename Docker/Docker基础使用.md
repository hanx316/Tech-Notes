# Docker基础使用

## 镜像和容器操作常用命令

```
# 搜索关键词相关镜像
docker search <name>

# 搜索信息过滤，例如：搜索 star 30以上的 ubuntu 镜像
docker search -f=stars=30 ubuntu

# 拉取镜像，默认拉取 tag 为 latest 的镜像
docker pull <name>

# 拉取指定 tag 的镜像
docker pull <name>:<tag>

# 列出所有本地镜像
docker images

# 列出所有本地镜像，包括隐藏镜像
docker images -a

# 列出所有容器id
docker images -q

# 删除镜像
docker rmi <image-id> <image-id> ...

# 删除所有镜像
docker rmi $(docker images -q)

```
