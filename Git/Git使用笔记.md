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