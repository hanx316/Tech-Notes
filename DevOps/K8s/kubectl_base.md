# kubectl 使用备忘

kubectl 是官方提供的用于访问 k8s 集群的命令行工具，平时个人主要用来查看服务发布运行情况和 pod 的日志，当然它还有其他功能，但个人手动用到的时候很少。

下面做一些备忘笔记。

## 帮助

遇事不决就用 `--help`。

[官方 kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## 配置

```bash
# 查看当前关联的集群配置信息
kubectl config view
```

kubectl 的配置信息存在 `$HOME/.kube` 目录下（MacOS），将访问配置写入 config 即可。

也支持写入多个集群的配置信息，参看 yaml 配置中的 `clusters` `contexts` 和 `users`。

其中各项配置的 `name` 字段可以根据需要任意修改，保持本地可以正确关联，不重复即可。

```bash
# 查看上下文
kubectl config get-contexts

# 切换上下文
kubectl config use-context CONTEXT_NAME
kubectl config use CONTEXT_NAME
```

## 查看服务

基本上就是通过 get 获取各类资源信息，一般会加上 namespace 参数，否则会使用 default namespace。

```bash
# 查看服务
kubectl get service -n NAMESPACE
kubectl get svc -n NAMESPACE

# 查看 deploy
kubectl get deployment -n NAMESPACE
kubectl get deploy -n NAMESPACE

# 查看 pod
kubectl get pods -n NAMESPACE
kubectl get po -n NAMESPACE
```

更多资源信息可以使用 `kubectl api-resources` 查看。

## 日志

主要是查看 pod 的日志信息时的一些操作。

```bash
# dump 某个 pod 最近 100 行日志，然后持续输出日志，不加 tail 会先 dump 所有日志出来
kubectl logs -f POD_NAME --tail 100 -n NAMESPACE

# dump 某个 pod 最近 500 行日志到一个临时文件(logs)
kubectl logs POD_NAME --tail 500 -n NAMESPACE > logs

# dump 某个服务的所有 pod 的最近 500 行日志到一个临时文件(logs) APP_NAME 是发布服务时 yaml 中配置的 label
# 目前使用中发现拉取所有 pod 的日志时必须指定 tail 否则默认每个 pod 取十行；并且 tail 是针对单个 pod 而言，比如有 4 个 pod 那么实际取得的日志是 2000 行
kubectl logs -l app=APP_NAME -n NAMESPACE --tail 500 > logs

# 取某个 pod 最近一小时的日志
kubectl logs POD_NAME -n web --since 1h > logs
```

## 进入 pod 的容器

前提是运行的容器支持 `sh` 或者 `/bin/bash`，一般都有。

```bash
# pod 只有单个容器
kubectl exec -it POD_NAME -n NAMESPACE -- sh
kubectl exec -it POD_NAME -n NAMESPACE -- /bin/bash

# pod 有多个容器
kubectl exec -it POD_NAME -c CONTAINER_NAME -n NAMESPACE -- sh
```
