# Git安装与配置

### Windows 系统安装 Git

Windows 系统安装 Git 可以直接用这个[ Git for Windows ](https://git-for-windows.github.io/)，也叫 `msysgit`，自带了 Git 命令行工具和一个图形界面工具

安装的时候可以选择添加 Git 到环境变量，方便在 CMD 使用

还有一个[乌龟 Git(TortoiseGit) ](https://tortoisegit.org/)可视化操作软件，虽然日常都用命令行，但是也可以装上，搭配 **Beyond Compare** 在比对文件版本差异时很方便

### Macos 安装 Git

Mac上推荐先安装 `homebrew`，然后 `brew install git` 即可，git会被安装到 `/usr/local/cellar` 目录下

mac下会自动生成一个 `.DS_Store` 的文件，用于描述文件的一些位置信息之类的，可以选择在 Mac 下设置不生成。如果没有设置的话，这个文件也不用提交到 Git，需要 Git 设置全局忽略该文件

```bash
echo .DS_Store >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

### Git配置

```bash
# 查看当前仓库配置信息
git config --list(-l)

# 查看全局配置信息
git config --global -l

# 设置用户名
git config --global user.name hanx

# 设置邮箱
git config --global user.email hanx@mail.com

# 设置提交 git 时自动将换行符转化为 lf
git config --global core.autocrlf input

# Git 提交默认对文件名的大小写不敏感，如果文件引用对大小写敏感的话，可以开启该项
git config core.ignorecase false

# 删除配置
git config (--global) --unset user.name
```

Windows 系统下的换行默认是 CRLF，Linux 和 Mac 下的换行的则是LF，这是一个坑，比如 shell 脚本在 Windows 下保存为 CRLF 换行的话，执行起来就会报错，而光从代码上看是完全正确的

Windows 一般使用 git for windows, 通常不会发现中文显示有问题，Mac 在终端使用 Git 的时候中文文件名可能会显示 unicode 编码，通常进行如下设置即可

```bash
# 不对 0×80 以上的字符进行 quote，中文显示正常
git config --global core.quotepath false
```

### 公钥和私钥

这一点主要涉及到本地和远程仓库的通信问题，一般来讲远程仓库要判断用户访问并获取代码的权限可以通过用户名密码的方式，不过这样验证一来需要频繁输入显得麻烦，二来也有密码暴露的风险

Git 使用了 SSH 协议，这种协议主要用于本地计算机和远程计算机之间的通信，参考[ SSH 原理与运用 - 阮一峰](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)

简单说就是，公钥可以随意暴露，而私钥则存在用户自己手中不能泄露，用于二次加密和验证，同时也免去了输入密码的步骤

```bash
# 以自己的邮箱设置密钥，首次执行会让输入密码之类的，可以直接跳过
ssh-keygen -t rsa -C hanx@mail.com

# 到.ssh目录下查看密钥文件和公钥
cd ~/.ssh
ls
cat ~/.ssh/id_rsa.pub
```

首次使用可以直接创建，Windows 系统会在默认的用户路径下（例如 C:\users\xxx） 新建 `.ssh\id_rsa和.ssh\id_rsa.pub` 两个文件，公钥存放在 `.pub` 文件中

如果已经有了密钥也可以再次生成，选择覆盖掉，也可以保存备份起来，以便将来重装系统或者其他情况可以复用旧的公私钥

私钥不用管它，公钥就是我们需要提供给远程仓库管理者添加访问权限了，以 Github 为例，在 setting 里边添加 `ssh key` 就能将自己的本机和 Github 关联起来
