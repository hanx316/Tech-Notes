# Docker介绍和安装

## Docker简介

Docker 最初是 dotCloud 公司创始人 Solomon Hykes 在法国时的一个公司内部项目，使用 Go 语言开发实现，基于现代 Linux 内核的 control group，namespace，以及 AUFS 类的 Union FS 等技术，对进程进行封装隔离，属于操作系统层面的虚拟化技术。由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。最初实现是基于 LXC, 从 0.7 版本以后开始去除 LXC, 转而使用自行开发的 libcontainer, 从 1.11 开始，则进一步演进为使用 runC 和 containerd。

Docker 于 2013 年 3 月以 Apache 2.0 授权协议开源，开源之后一路高歌猛进，dotCloud 公司甚至将公司名字改为了 Docker。

Docker 在容器的基础上，进行了进一步的封装，从文件系统、网络互联到进程隔离等等，极大的简化了容器的创建和维护。使得 Docker 技术比虚拟机技术更为轻便、快捷。

## Docker与VM

传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

Docker 与传统的 VM 相比，它更轻量，启动速度更快，单台主机上可以同时运行成百上千个容器。

## macOS上安装Docker

Mac 上安装和使用 Docker 非常简单，参考官方文档 [Install Docker for Mac](https://docs.docker.com/docker-for-mac/install/) 下载 Docker for Mac 即可，体验很好。
