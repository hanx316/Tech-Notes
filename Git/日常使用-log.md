## 日常使用 - 查看记录日志

```bash
# 查看提交记录日志
git log

# 查看操作记录日志
git reflog

# 一条推荐使用的日志查看命令 可以建一个别名方便使用
git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short'
```