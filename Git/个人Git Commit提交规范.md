# 个人Git Commit提交规范

自己的Git Commit提交规范，也包括push, branch等操作，慢慢完善

1. commit之前确保status已经up to date

2. 首次push须加上`-u`参数设置好仓库关联

3. commit必须带上`-m`参数填写提交信息，须以方括号加上标签关键词表明提交类别，说明如下

  - **[feat]** feature 新增功能
  - **[fix]** fix 修复问题
  - **[doc]** document 文档增删改
  - **[conf]** config 配置增删改
  - **[form]** format 代码等文件格式，排版等修改
  - **[ref]** refactor 不涉及新增功能和修复问题的代码重构

4. 一次commit如包含多个分类提交信息，须按类别逐项列出，如`[feat]新增A功能 [conf]修改B配置`