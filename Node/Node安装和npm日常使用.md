# Node.js学习笔记1

## 安装Node.js

### 安装包安装固定版本

截止到我写这篇笔记的时候，Nodejs的正式发布版本已经到了9.2.1，长期稳定支持版本（LTS, long time support）是8.9.3。整个node的生态圈已经非常繁荣了。像安装这样的问题，官方也早已经给出了非常方便的方式。

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

# 安装固定版本的包
npm i <name@version>

# 写入安装信息到package.json的dependencies
npm i <name> --save(-S)

# 写入安装信息到package.json的devdependencies
npm i <name> --save-dev(-D)
```

官网及镜像链接

[npm官网](https://www.npmjs.com)

[npm文档](https://docs.npmjs.com)

[npm淘宝镜像](https://npm.taobao.org/)

推荐使用nrm来管理和切换源

```bash
# 全局安装nrm
npm i nrm -g

# 查看版本
nrm --version

# 列出所有当前源
nrm ls

# 使用某个源
nrm use cnpm
```

### npm初始化以及package.json

```bash
# npm初始化一个模块/项目
npm init

# npm初始化一个模块/项目，并带上基本模板信息
npm init -y
```

初始化一个模块或者项目会需要填写诸如项目名称，作者，关键词等等，最后会生成一个package.json文件，用以提供模块/项目的相关信息，并且还有一些非常实用的字段。我通常在开启一个新项目/模块的时候，会用`npm init -y`生成基本的package文件模板，然后根据需要修改其中的内容。

下面介绍其中常用到的几个字段

- dependencies和devDependencies

  在刚接触npm的时候常常搞不懂`npm install xxx --save-dev`和`npm install xxx --save`还有`npm install xxx`有什么区别。其实单纯从安装的角度来说，并没有什么区别。其区别主要是在写入package.json文件的字段不同。
  
  低版本的npm，不带任何参数执行install，并不会将安装模块的信息写入到package.json，所以是不被采纳的做法。目前npm5似乎是不带参数执行也会默认将安装的模块写入到依赖字段（dependencies），这和带上`--save`或者`-S`参数的效果一样，表明该项目的运行需要依靠这些包。比如用到的框架，工具类库，插件等在实际项目内部的模块就会写到这里。
  
  devDependencies顾名思义就是本地的依赖，就是说当前程序要在本地运行和开发，需要依靠这些包。带上`--save-dev`或者`-D`参数就是在安装依赖时将其写入到这个字段。比如本地构建相关的包通常就会写入到这里。

  严格区分所安装模块的依赖和本地依赖关系，以便自己和他人在阅读package.json获取项目信息时能更有条理，一目了然。对使用者而言，如果需要阅读相关代码，也只需要关注dependencies。

  对开发者而言，运行`npm i`会安装package.json文件中dependencies和devDependencies的所有依赖，以便项目能够在本地顺利运行。

  此外还有一个peerDependencies字段，但是我还没有在项目中用到，也未有见到过，这里就不提了。

- scripts字段

  这个字段就是npm为我们提供了一个非常好用的工具链手段，npm scripts。我们知道可以通过在命令行执行`node xxx`，让node去运行当前目录下的xxx.js文件，但是如果命令带上一些复杂的参数，每次输入就很烦了，可以在这里把命令都添加进去，而在开发中直接运行`npm run xxx`就能够执行了，当然这只是npm scripts的一个小小的基本用法。