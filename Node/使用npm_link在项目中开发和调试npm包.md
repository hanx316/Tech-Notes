# 使用 npm link 在项目中开发和调试 npm 包

[npm link 文档](https://docs.npmjs.com/cli/link.html)

将依赖包 link 到项目中之后就可以在开发环境对依赖包进行修改，然后在项目中调试。

# link 依赖包

```bash
# 在开发的包目录下运行 npm link 会在全局模块目录下创建软连接到这个包目录
cd ~/some_package
npm link

# 在项目中 link 依赖包，注意包名取 package.json 中的配置
cd ~/some_project
npm link some_package
```

也可以省略 link 全局的步骤

```bash
# 在项目目录中 link 包的目录，会自动先创建全局 link 然后再
cd ~/some_project
npm link ~/some_package
```

# unlink 依赖包

```bash
# 删除项目的依赖包 link
cd ~/some_project
npm unlink some_package

# 删除依赖包的全局 link
cd ~/some_package
npm unlink
```


