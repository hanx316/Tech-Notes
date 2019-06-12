# 使用 Minikube 创建 mac 本地 k8s 单节点实验环境

k8s 官方提供了开箱即用的 [Minikube](https://github.com/kubernetes/minikube) 来在本地快速创建一个单节点的 k8s 集群。

然而由于万恶的 GFW， 这么方便的工具在使用的时候依然困难重重，折腾了许久之后最终还是放弃通过官方途径安装和构建，墙内推荐使用[阿里云修改的 Minikube](https://github.com/AliyunContainerService/minikube)。

阿里云的版本会相对官方滞后一些，但是只是拿来实验用，所以无伤大雅。理论上，它应该只将其中拉取虚拟机镜像等文件的地址从谷歌换到了阿里云，毕竟折腾的最主要问题就是网络原因，开代理也是碰运气。

> Minikube 的本质其实是将一套 “定制化” 的 K8S 集群打包成 ISO 镜像，当执行 minikube start 的时候，便通过此镜像启动一个虚拟机，在此虚拟机上通过 kubeadm 工具来搭建一套只有一个节点的 K8S 集群。

## 准备工作

需要先安装 vm-driver， mac 支持：xhyve driver, VirtualBox 或 VMware Fusion

推荐安装 VirtualBox, 启动时默认也使用这个。可以手动指定 `--vm-driver=xxx` 或者 `--vm-driver=none`， 为 none 时需要安装 Docker

kubectl 可以预先安装，也可以不用，现在的 Minikube 版本应该在安装时同时安装 kubectl

如果安装过 Docker for mac， Docker 会自带 kubectl， 但是版本较低，需要安装 kubectl 之后手动修改一下文件连接。

推荐使用 Homebrew 安装 kubectl

```bash
# 安装
brew install kubernetes-cli
# 修改连接
brew link kubernetes-cli --overwrite
# 安装成功确认
kubectl version
```

## 安装 Minikube

使用阿里云的 Minikube 进行安装，地址上的版本号可以查看 github 修改。

安装之前确保 kubectl 版本和 Minikube 版本不要有大版本号的差距。

```
curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v1.1.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

# 安装成功确认
minikube version
```

## 启动集群及常用命令

```bash
# 启动
minikube start
# 推荐手动指定拉集群取镜像的源地址
minikube start --registry-mirror=https://registry.docker-cn.com

# 状态查看
minikube status

# 关闭集群
minikube stop

# 进入虚拟机节点
minikube ssh

# 删除集群
minikube delete
```

minikube 启动后会自动配置好 kubectl 的 config, 可以通过 kubectl 访问集群。

## 参考：

- [Minikube - Kubernetes本地实验环境](https://yq.aliyun.com/articles/221687?spm=a2c4e.11153940.blogcont221687.154.7dd54cec1yQlPe&p)

- [掘金 k8s 小册](https://juejin.im/book/5b9b2dc86fb9a05d0f16c8ac/section/5b9b81735188255c8b6edc28)
