# 个人 Git Commit 提交规范

自己的 Git Commit 提交规范，也包括 push, branch 等操作，慢慢完善

1. commit 之前确保 status 已经 up to date

2. 首次 push 须加上 `-u` 参数设置好仓库关联

3. commit 必须带上 `-m` 参数填写提交信息，基本格式 `<type>(<scope>): <subject>`, scope 作为可选项一般不写

type 说明：
- feat: feature 新增功能
- fix: 修复 bug
- doc: 文档修改
- config: 配置或者构建等文件增删改
- style: 代码风格修改
- refactor: 不涉及新增功能和修复问题的代码重构，以及目录结构的调整

4. 尽量避免一次提交多个不同类型的修改

5. [阮一峰博客关于 git commit message 的介绍](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)