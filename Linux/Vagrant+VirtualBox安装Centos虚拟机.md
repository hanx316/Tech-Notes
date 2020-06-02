# Vagrant + VirtualBox 安装 Centos 虚拟机

操作笔记，选择 VirtualBox 当然是因为它免费啊！

## 安装 Vagrant

[Vagrant 官网](https://www.vagrantup.com/) 下载安装，升级可以下载新的包直接安装覆盖，也可以先卸载再安装。

安装完成后执行 `vagrant --version` 检查是否成功。

## 安装 VirtualBox

[VirtualBox 官网下载](https://www.virtualbox.org/wiki/Downloads)，选择对应的平台，确保安装包和扩展安装包版本一致，升级也需要保证一致。

VirtualBox 提供了应用程序可以通过可视化界面创建和管理虚拟机，但是这些操作我们都通过 Vagrant 来完成， VirtualBox 仅提供背后的支持。

macOS 10.13 之后由于安全策略可能会导致安装失败，删除已安装的内容之后，在系统偏好设置的安全性与隐私里面点一下允许然后重新安装即可。

## 创建和管理 Centos 虚拟机

关于 vagrant 的使用可以查阅 [Vagrant 官方文档](https://www.vagrantup.com/docs/) 以及 `vagrant --help` 命令。

这里还有一个 [入门参考](https://github.com/whorusq/learning-vagrant)

下面是基础使用：

```bash
mkdir centos7 && cd centos7

# 生成 Vagrantfile，描述虚拟机的一些配置
vagrant init centos/7

# 找到对应的 vagrant box 创建和启动虚拟机，如果没有对应系统的 vagrant box 会到默认的官方站点下载，这一步比较耗时
# 所以通常会事先将 box 下载好然后添加到 vagrant
# centos7 文件不大，可以直接操作，创建完成的虚拟机可以打开 VirtualBox 看到
vagrant up

# 通过 ssh 进入虚拟机
vagrant ssh
# 退出虚拟机
exit

# 查看所有创建的虚拟机状态
vagrant status
# 关闭开启的虚拟机
vagrant halt
# 删除创建的虚拟机
vagrant destroy
```

## 启动报错处理

2020.06.02 更新。

有一段时间没有用 vagrant 了，今天启动虚拟机突然发现报错无法启动。错误如下

```
There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.

Command: ["startvm", "～～～ 马赛克 ～～～", "--type", "headless"]

Stderr: VBoxManage: error: The virtual machine 'centos7_default_1591109834119_4986' has terminated unexpectedly during startup with exit code 1 (0x1)
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component MachineWrap, interface IMachine
```

尝试在 VirtualBox 手动启动虚拟机也失败，提示没有 linux 内核之类的巴拉巴拉。推测可能由于系统更新或者关机重启之类的导致 VirtualBox 出了问题。

在 stackoverflow 上找到一个[答案](https://stackoverflow.com/questions/54286789/mac-there-was-an-error-while-executing-vboxmanage-a-cli-used-by-vagrant)，成功解决。

```bash
sudo "/Library/Application\ Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh" restart
```

看上去应该就是重启一下 VirtualBox 的一些守护进程吧。
