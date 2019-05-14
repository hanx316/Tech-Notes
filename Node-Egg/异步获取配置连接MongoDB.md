# Egg.js(v2.22.2) 异步获取配置连接 MongoDB

这里主要使用 egg-mongoose 插件来连接 MongoDB, 使用方式可查看 [egg-mongoose README](https://github.com/eggjs/egg-mongoose)。

需要特别指明版本是因为 Egg.js 在某个版本改变了启动自定义的方式，过去的 `beforeStart` 等方法仅作为兼容保留。[启动自定义文档](https://eggjs.app/zh-cn/basics/app-start.html)

新版的 `configWillLoad` 钩子是在应用层动态修改配置文件的最佳办法，但是这个方法目前只支持同步调用，而我们需要异步获取数据库连接配置再去修改 egg 的 config, 这样一来就无法使用这个钩子函数了。

从 issue 看来官方应该是有计划在 3.x 做异步配置的支持，所以这里基于当前使用的版本来说明。

## 探索思路

如果不能在 `configWillLoad` 里做，那能不能在其他地方做呢？我设想了三个方向的方案：

1. 回退到 `beforeStart` 方法，注册 async 函数

    这个办法作为兜底方案，因为目前 egg 还保留着旧版方法作为兼容，但是不到万不得已最好不用

2. 试试看新版的其他钩子

    具体而言，egg-mongoose 是一个插件，理论上我们可以在插件启动完成之前获取到配置手动注入，但是这种方式只是理论上，而且也存在配置和插件加载时序错乱的风险

3. 直接改写 egg-mongoose 插件，在插件内异步获取配置，相当于配置默认化

    实际上这是我在 `configWillLoad` 无果之后首先尝试的方案，然而这个方案也有几个问题：首先是改写插件把问题扩大了，问题变大成本也就变大；其次每个应用服务需要的配置应该是定制化的，在插件只能获取所有配置；最后，如何让插件知道应用层需要的配置也是一个要考虑的问题。

既然初步准备改写 egg-mongoose 插件，那么自然要看一看它的源码，同时[官方的插件编写文档](https://eggjs.app/zh-cn/advanced/plugin.html)也要参考，意外的是这个插件文档反而让我找到了最终的解决办法。

## egg-mongoose 做了什么

这个插件其实做了两件事情，一是对 Mongoose 连接 MongoDB 进行了封装，将 `mongoose` 对象挂载到 `app.mongoose`，以及将插件服务自身挂载到 `app.mongooseDB`; 二是将项目中 model 目录下的内容注册到应用上，这一点非常关键。

上面提到了插件服务，其实是指官方插件编写文档中的 `addSingleton` 创建的对象，这个下面再说。

## 解决方案

官方的插件编写文档以 egg-mysql 为示例，其中就提到了[异步获取数据库配置然后初始化服务的方法](https://eggjs.app/zh-cn/advanced/plugin.html#%E6%8F%92%E4%BB%B6%E5%86%99%E6%B3%95)，但是这是插件内部做的事。

这里说点题外话，其实可以看到这个插件也好，egg-mongoose 也好，内部其实都是通过 `app.beforeStart` 来完成启动前初始化的，文档如此，可以想见，广大类似插件多半也都是使用这个 api。我不知道插件的这个钩子方法和应用层的钩子方法有什么区别，但是应用层的钩子改了，插件这边如果要配合改变的话，这个波及的面就挺广的了。

其实就在插件编写文档的应用层使用方案有提到[动态创建实例的方法](https://eggjs.app/zh-cn/advanced/plugin.html#%E5%8A%A8%E6%80%81%E5%88%9B%E5%BB%BA%E5%AE%9E%E4%BE%8B)，虽然文档是 mysql 的插件，但其中核心的 [addSingleton](https://github.com/eggjs/egg/blob/2.22.2/lib/egg.js#L458) [createInstance](https://github.com/eggjs/egg/blob/2.22.2/lib/core/singleton.js#L82) [createInstanceAsync](https://github.com/eggjs/egg/blob/2.22.2/lib/core/singleton.js#L91) 这些方法都是框架提供的，看到这里，我们的方案也就调整为**不让 egg-mongoose 初始化连接数据库，而是异步获取到配置之后我们动态创建连接服务的实例**，而正好除了 `configWillLoad` 其他钩子函数都支持异步调用。

不让 egg-mongoose 初始化连接数据库我们不传入任何相关配置即可，插件内部做了相应的容错处理。需要注意的是我们还得关掉插件对 model 的自动加载，这一点插件的文档并没有说明。

```js
config.mongoose = {
  loadModel: false,
}
```

如果不关掉自动加载，那么在应用启动时没有连接数据库但又加载 model 就会出错。

接下来就是在应用启动钩子函数中选择一个合适的异步方法来执行异步获取配置并创建实例了，从提供的新版钩子来看，`didLoad` 可以用于启动自定义服务，而 `willReady` 在所有插件启动完毕之后调用，似乎后者更合适一些，就我实测来看，这两个方法都是可以使用的，也许插件启动完毕是指插件自己创建的服务完成，而实际上动态创建服务只依赖插件加载完毕？

```js
class AppBoot {
  constructor(app) {
    this.app = app
  }

  async willReady() {
    const dbConfig = await getConfig()
    const DB = await this.app.mongooseDB.createInstanceAsync(dbConfig)
    this.app.mongooseDB.clients.set('someDB', DB)
    await this.app.mongoose.loadModel()
  }
}
```

egg-mongoose 插件将 `addSingleton` 创建的对象改写到 `app.mongooseDB` [参见](https://github.com/eggjs/egg-mongoose/blob/master/lib/mongoose.js#L33)

将 `app.mongoose` 改成了 mongoose 本身 [参见](https://github.com/eggjs/egg-mongoose/blob/master/lib/mongoose.js#L42)

然后将加载 model 的方法 `loadModel` 暴露到 `app.mongoose` 上 [参见](https://github.com/eggjs/egg-mongoose/blob/master/lib/mongoose.js#L46)
