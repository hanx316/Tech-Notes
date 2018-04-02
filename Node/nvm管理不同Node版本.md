# nvm管理不同Node版本

## 安装

github地址：https://github.com/creationix/nvm

执行下面的安装脚本，最新版本号以github上的为准

```curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash```

安装完成也许可能要配置环境变量之类，我没有遇到，安装即可使用

`nvm --version` 查看版本号，检查是否安装成功

## 基本使用

```bash
# 安装当前最新版本node
nvm install node

# 安装当前最新lts版本
nvm install --lts

# 安装指定node版本，模糊版本号会安装该系列最新版本
nvm install <version>
# 安装4打头的最新版本
nvm install 4

# 卸载node
nvm uninstall <version>
nvm uninstall --lts

# 查看当前安装的所有node，也可以看到当前使用的是哪个版本
nvm ls

# 安装成功后的node版本会默认use，可以查看当前正在使用哪个版本
nvm current

# 使用指定版本的node
nvm use <version>

# 使用当前最新版本node
nvm use node

# 使用当前最新lts版本
nvm use --lts

# 查看node版本的可执行文件
nvm which <version>
```

## 关联默认版本

每次打开终端，启用的都是标识为default的node版本，这个可以通过别名来设置

```bash
nvm alias default <version>

# 也可以设置其他别名方便快速切换
nvm alias <name> <version>
```