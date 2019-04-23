# Mongo Shell 基本操作

记录一些常用的操作命令，不涉及配置参数等。

## 基本命令

```bash
# 查看当前连接下所有数据库
show dbs

# 查看当前数据库
db

# 切换当前数据库，如果 db 不存在也可以执行切换，在自动创建集合的时候会创建 db
use <db>

# 查看当前数据库所有集合
show collections

# 帮助命令
help

# 退出
exit
```

## 插入数据

在输入命令时没有闭合括号前可以任意换行以便书写。

```bash
# 插入单条文档，如果集合不存在会自动创建
db.<collection>.insertOne(<document>)

# 插入多条文档
db.<collection>.insertMany([<document>, ...])

# 既可以插入单条文档，也可以插入多条文档
db.<collection>.insert(<document> or [<document>, ...])

# 使用 save 插入新文档时实际调用 insert
db.<collection>.save(<document> or [<document>, ...])
```

`insertOne` 和 `insertMany` 以及 `insert` 三者区别：

1. 返回结果对象格式不一样

`insert` 无论成功失败都不会抛错；插入单条文档，返回一个 `WriteResult` 对象，显示插入了多少条数据，如果发生错误会包含 `writeError` 字段；插入多条文档，返回一个 `BulkWriteResult` 对象，如果发生错误会包含 `writeErrors` 字段

`insertOne` 和 `insertMany` 插入成功之后会返回一个普通对象包含插入的文档主键以及写入的安全级别

`insertOne` 插入失败会抛出 `WriteError` 错误， `insertMany` 插入失败抛出 `BulkWriteError` 错误

2. `insertOne` 和 `inserMany` 不支持 `db.<collection>.explain()` 命令

### 插入示例

```bash
use test
db.users.insertOne({
  name: 'alice',
  age: 18,
})
show collections
db.users.insertMany([
  {
    name: 'bob',
    age: 20,
  },
  {
    name: 'charlie',
    age: 20,
  },
])
```
