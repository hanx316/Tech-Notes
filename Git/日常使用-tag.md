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

# 查看当前分支 (HEAD) 包含的最近的一个 tag
# describe 命令不带 tag/all 参数 会返回最近一条带有注释 message 的 tag
git describe
git describe --tags/--all
git describe --tags --abbrev=0 # 确保只显示 tag 名称

# 查看当前仓库的最新 tag 的 commit-ish
git rev-list --tags --max-count=1

# 查看当前仓库的最新 tag (基于仓库最新 tag 的 commit-ish 查看)
git describe --tags `git rev-list --tags --max-count=1`

# checkout 当前分支 (HEAD) 最新 tag
git checkout $(git describe --tags --abbrev=0)

# checkout 当前仓库最新 tag
git checkout $(git rev-list --tags --max-count=1)
```
