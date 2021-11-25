# Egg框架基础项目搭建

使用 `egg-init` 可以生成一个项目模板，不过通常都会自己手动来搭建项目，一方面更加定制化且更可控，另一方面也可以熟悉整个流程。

[官网的 Quick Start](https://eggjs.app/en/intro/quickstart.html) 是一份很好的指引文档，本文主要是依据该文档搭建项目的实践笔记。

## 安装基础依赖

一个最简单的 Egg 项目需要安装两个包，作为框架的 `egg` 和用作本地开发的 `egg-bin`。

```bash
npm i egg -S
npm i egg-bin -D
```

## 基本项目结构

Egg 基本上是按照 MVC 结构来组织代码的，要跑起来一个最简单的 Egg 项目，我们需要添加 Controller, Router, 以及配置 Security Cookies Key。

文件结构：

```bash
project folder
├── app
│   ├── controller
│   │   └── home.js
│   └── router.js
├── config
│   └── config.default.js
└── package.json
```

### 控制器

控制的写法大同小异，个人偏向下面这样。

```js
class HomeController extends require('egg').Controller {
  async index() {
    this.ctx.body = 'test'
  }
}

module.exports = HomeController
```

### 路由

路由的 url 可以支持正则表达式，这里我希望 `/` 和 `/index` 路径都打开首页。另外，其实不用 router，直接在 app 上调用路由的 verb 方法也是可以的。

```js
module.exports = app => {
  const { router } = app

  router.get(/^(\/|\/index)$/, 'home.index')
}
```

### 配置

配置文件如果需要访问应用信息，可以写成函数的形式，如果不需要，则可以直接 exports 一个配置对象。

自己搭项目常常会忘记配置 Security Cookie Key 这一步。

```js
module.exports = appInfo => {
  return {
    keys: 'JUST_FOR_FUN',
  };
};
```

### 运行

通过 `egg-bin dev` 就可以让这个最简单的项目在本地跑起来了。

更多关于 `egg-bin` 的使用可以参考该项目仓库的[文档](https://github.com/eggjs/egg-bin)

```json
"scripts": {
  "dev": "egg-bin dev"
}
```
