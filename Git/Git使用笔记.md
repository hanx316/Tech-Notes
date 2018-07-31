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

# 修改上一次提交的 message
# 通常用在修改了本地提交但未 push 到远程仓时，如果已经 push, 就要考虑是否能够重写 commit message 了
git commit --amend
git commit --amend -m 'message'
# 如果可以重写，且本地仓库与远程仓库保持一致，那么可以强制 push 重写远程仓的历史
git push -f

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