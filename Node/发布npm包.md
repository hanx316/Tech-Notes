# 发布和管理 npm 包

最近在公有源和公司的私有源都发了几个 npm 包，所以专门记录一下过程。

## 发布到公有 npm

一般我们都会切换到淘宝源等墙内方便下载的 registry, 发布之前一定记得切换到 `https://registry.npmjs.org/`。

发布流程比较简单：

```bash
# 输入 https://www.npmjs.com 的账号密码和邮箱
npm login

# 在要发布的 package 根目录执行即可
npm publish
```

包的名字总是先到先得，如果名字重复可以发布 scoped package， 比如 `@xxscope/xxpackage`

```bash
# 其实就是改变 package.json 里的 name
npm init --scope=@xx -y

# scoped packages 默认以 private 形式发布，然而这要收费，所以需要特别指定公开访问
npm publish --access public
```

## 发布到私有 npm

流程基本和公有 npm 一致，使用细节上就看用什么来搭了，就目前公司用到的两个私有 npm 来看，有些差别：

- 使用统一账号，然后可能不用或者随便输入邮箱

- scoped package 发布不用指定 public

- 发布和公有源上相同名字的包时，可以发布成功，但下载仍然会从公有源下载，除非将包的版本号设为公有源上不存在的版本（这点和我想象的可以同名区分有很大不同）

## 版本号管理

关于版本号的原则可以参考 [Semver](https://semver.org/lang/zh-CN/)， 一般主要关注 `major/minor/patch` 三个版本形式就够了。

npm 提供了一系列命令可以很方便地联合 git 来完成版本号升级：

```bash
# 变更对应的 patch/minor/major 版本号，并根据 commit message 提交 commit， 然后打上相应的 tag(vx.y.z), 其中 %s 是生成的版本号
npm version patch/minor/major -m 'realease: %s'
```

## 使用 .npmignore

很多时候并不需要将代码版本库中的所有文件都发布到 npm， `.npmignore` 文件就派上了用场。

npm 会默认忽略一些文件，以及有一些文件无法配置忽略，这些可以在 [参考文档](https://docs.npmjs.com/misc/developers#keeping-files-out-of-your-package) 查阅。
