# Node.js学习笔记1

## 安装Node.js

### 安装包安装固定版本

截止到我写这篇笔记的时候，Nodejs的正式发布版本已经到了9.2.1，长期稳定支持版本是8.9.3。整个node的生态圈已经非常繁荣了。像安装这样的问题，官方也早已经给出了非常方便的方式。

Windows和macOS安装node都可以直接在[Nodejs官网](http://nodejs.org/zh-cn)上下载安装包，无脑下一步安装。

Windows下载对应bit的.msi安装包，macOS下载.pkg即可。可以通过下载新版本的安装包进行更新node版本。

安装成功之后在命令行执行`node -v`可以查看node安装的版本。

### 二进制文件安装

目前还没有用到这种方式，以后遇到再总结。

### Node版本管理

可以通过nvm和n这两个工具来管理node版本。

nvm只能运行在mac下，windows下可以使用nvmw。

nvm的安装和配置就稍微复杂一点，但是可以让各个版本的Node运行在独自的沙箱中，每一个版本的Node模块都相互独立（全局模块）。

n则是node的一个模块，安装好node以后全局安装n模块就可以进行node版本的切换，不过n无法干涉全局模块。

我暂时打算采用n来进行node版本管理，因为它足够轻量，并且需要我切换的场景也不是太多。

附淘宝FED博文[管理node版本，选择nvm还是n？](http://taobaofed.org/blog/2015/11/17/nvm-or-n/)作为参考。

## npm日常使用

npm全称node package manager，是node官方推荐的node模块（包）管理工具。如今安装node已经同时安装上了npm，`npm -v`可以查看安装的版本。

npm安装的包都在node_modules目录内，当前路径直接新建，全局安装以mac为例，安装在`/usr/local/lib/node_modules`目录下。全局安装的路径也可以进行修改。

```bash
# 全局安装更新npm到最新版本，mac终端提示权限问题可以前面加上sudo
npm i -g npm

# 查看npm配置
npm config list --json

# 查看当前镜像 默认 https://registry.npmjs.org/
npm config get registry

# 设置npm镜像为淘宝源（如果发包的话注意改回原来的）
npm config set registry http://registry.npm.taobao.org

# 全局安装包
npm install(i) <name> -g

# 当前路径安装包
npm i <name>

# 删除包
npm uninstall <name>

# 全局删除包
npm uninstall <name> -g
```

[npm官网](https://www.npmjs.com)

[npm文档](https://docs.npmjs.com)

[npm淘宝镜像](https://npm.taobao.org/)