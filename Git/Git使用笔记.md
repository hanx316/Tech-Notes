# Git使用笔记

与集中式管理的svn不同，git作为分散型版本管理的代表，流程上要复杂很多。不过就像玉伯说Vue是先甜后苦，React是先苦后甜一样，世间难易之事大抵如此吧，在使用过svn和git之后，我倒也是觉得svn是先甜后苦，git是先苦后甜

## Git安装

### windows平台

windows平台安装git可以直接用这个[git for windows](https://git-for-windows.github.io/)，也叫`msysgit`，自带了git命令行工具和一个图形界面工具

安装的时候可以选择添加git到环境变量，方便在cmd使用

还有一个[乌龟Git(TortoiseGit)](https://tortoisegit.org/)可视化操作软件，虽然日常都用命令行，但是也可以装上，搭配**Beyond Compare**在比对文件版本差异时很方便

### macos平台

mac上推荐先安装`homebrew`，然后`brew install git`即可，git会被安装到`/usr/local/cellar`目录下

mac下会自动生成一个`.DS_Store`的文件，用于描述文件的一些位置信息之类的，可以选择在mac下设置不生成。如果没有设置的话，这个文件也不用提交到git，需要git设置全局忽略该文件

```bash
echo .DS_Store >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

## Git配置

```bash
# 查看当前仓库配置信息
git config --list(-l)

# 查看全局配置信息
git config -l --global

# 设置用户名
git config --global user.name hanx

# 设置邮箱
git config --global user.email hanx@mail.com

# 设置提交git时自动换行转化为lf
git config --global core.autocrlf input

```

windows平台下的换行默认是CRLF，Linux和Mac下的换行的则是LF，这是一个坑，比如shell脚本在windows下保存为CRLF换行的话，执行起来就会报错，而光从代码上看是完全正确的

windows一般使用git for windows, 通常不会发现中文显示有问题，mac在终端使用git的时候中文文件名可能会显示unicode编码，通常进行如下设置即可

```bash
# 不对0×80以上的字符进行quote，中文显示正常
git config --global core.quotepath false
```

## 公钥和私钥

这一点主要涉及到本地和远程仓库的通信问题，一般来讲远程仓库要判断用户访问并获取代码的权限可以通过用户名密码的方式，不过这样验证一来需要频繁输入显得麻烦，二来也有密码暴露的风险

git使用了ssh协议，这种协议主要用于本地计算机和远程计算机之间的通信，参考[SSH原理与运用 - 阮一峰](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)

简单说就是，公钥可以随意暴露，而私钥则存在用户自己手中不能泄露，用于二次加密和验证，同时也免去了输入密码的步骤

```bash
# 以自己的邮箱设置密钥，首次执行会让输入密码之类的，可以直接跳过
ssh-keygen -t rsa -C hanx@mail.com

# 到.ssh目录下查看密钥文件和公钥
cd ~/.ssh
ls
cat ~/.ssh/id_rsa.pub
```

首次使用可以直接创建，Windows系统会在默认的用户路径下（例如 C:\users\xxx）新建`.ssh\id_rsa和.ssh\id_rsa.pub`两个文件，公钥存放在.pub文件中

如果已经有了密钥也可以再次生成，选择覆盖掉，也可以保存备份起来，以便将来重装系统或者其他情况可以复用旧的公私钥

私钥不用管它，公钥就是我们需要提供给远程仓库管理者添加访问权限了，以github为例，在setting里边添加ssh key就能将自己的本机和github关联起来

## 日常使用 - 本地新建、添加、提交及关联并推送远程仓库

```bash
# 本地初始化一个git仓库
git init

# 添加指定文件修改记录到暂存区
git add <file>
git add <file> <file>

# 添加所有文件修改记录到暂存区
git add .

# 将当前暂存区的修改提交到本地仓库
git commit
git commit -m '提交说明'

# 如果只有单个文件可以合并操作
git commit -a -m '提交说明'

# 修改上一次提交的message，未push
git commit --amend

# 查看当前关联远程仓库信息
git remote -v

# 关联指定远程仓库
git remote add <仓库名，一般命名为origin> <仓库地址，http或者ssh>

# 克隆远程仓库
git clone <仓库地址，http或者ssh>

# 克隆远程仓库到指定目录
git clone <url> <directory>

# 本地修改推送到远程仓库
git push <远程仓库名，例如origin> <远程分支名，例如master>

# 本地推送到远程时进行分支关联
git push -u <repo> <branch>
```

`push`时如果省略仓库名，则默认`origin`

## 日常使用 - 查看记录日志

```bash
# 查看提交记录日志
git log

# 查看操作记录日志
git reflog

# 一条推荐使用的日志查看命令 可以建一个别名方便使用
git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short'
```

## 日常使用 - 分支操作

```bash
# 创建分支
git branch <branch-name>

# 切换到指定分支
git checkout <branch-name>

# 创建并切换到该分支
git checkout -b <branch-name>

# 关联本地分支与远程分支
git branch --set-upstream <branch-name> <origin/branch-name>

# 查看当前本地分支
git branch

# 查看当前所有分支(本地和远程) all
git branch -a

# 查看当前所有分支并附上提交信息
git branch -a -vv

# 合并指定分支到当前分支(在当前产生一个新的记录，其parent指向合并的两个分支)
git merge <branch-name>

# 将当前分支变基到指定分支(在指定分支产生一个新的记录，并且两条分支合并到一条时间线上)
git rebase <branch-name>

# 删除本地分支
git branch -d <branch-name>

# 强制删除本地分支，通常用于删除尚未合并过的分支
git branch -D <branch-name>

# 删除远程分支
git push origin :<branch-name>

# 如果删除了远程分支，而在另一台机器上没有同步信息可以执行
git fetch -p

```

## 日常使用 - 标签

```bash
# 给当前分支打标签
git tag <tagname>
git tag -a <tagname> -m '标签说明'

# 查看标签信息
git show <tagname>

# 删除指定标签
git tag -d <tagname>

# 推送标签到远程仓库
git push origin <tagname>

# 推送所有标签到远程仓库
git push origin --tags

# 删除远程仓库的标签
git push origin :refs/tags/<tagname>
```