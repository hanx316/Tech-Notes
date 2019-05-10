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

  - `insert` 无论成功失败都不会抛错；插入单条文档，返回一个 `WriteResult` 对象，显示插入了多少条数据，如果发生错误会包含 `writeError` 字段；插入多条文档，返回一个 `BulkWriteResult` 对象，如果发生错误会包含 `writeErrors` 字段

  - `insertOne` 和 `insertMany` 插入成功之后会返回一个普通对象包含插入的文档主键以及写入的安全级别

  - `insertOne` 插入失败会抛出 `WriteError` 错误， `insertMany` 插入失败抛出 `BulkWriteError` 错误

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

## 查询数据

主要使用 `db.<collection>.find(<query>, <projection>)` 命令，支持匹配查询与查询操作符号。

<query> 定义读取操作时筛选文档的条件，<projection> 定义对读取结果进行的投射。

find 命令返回文档查询的游标，游标可以进行进一步迭代和操作。

投射：控制查询返回的字段，节约系统资源，让查询更高效和灵活。

```bash
# 返回全部文档
db.restaurants.find()
db.restaurants.find().pretty()
```

### 匹配查询

```bash
# 单条件匹配
db.restaurants.find({ name: 'Time Out Lounge' })
# 多条键匹配
db.restaurants.find({ borough: 'Queens', cuisine: 'Armenian ' })
# 嵌套对象匹配
db.restaurants.find({ cuisine: 'American ', 'address.street': 'Broadway' })
# 嵌套数组匹配（只要数组中包含满足条件的元素）
db.restaurants.find({ 'grades.grade': 'B' })
```

### 筛选查询

<query>: ` { <field>: { $<operator>: <value> } }`

<field> 筛选的文档字段
$<operator> 比较操作符
<value> 比较的值

#### 比较操作符

- $eq - equal 匹配字段值相等的文档

- $ne - not equal 匹配字段值不等的文档

- $gt - greater than 匹配字段值大于查询值的文档

- $gte - greater than and equal 匹配字段值大于等于查询值的文档

- $lt - less than 匹配字段值小于查询值的文档

- $lte - less than and equal 匹配字段值小于等于查询值的文档

- $in - in 匹配字段值与任一查询值相等的文档 `{ <field>: { $in: [<value>, <value>, ...] } }`

- $nin - not in 匹配字段值与任一查询值不等的文档 `{ <field>: { $nin: [<value>, <value>, ...] } }`

```bash
db.restaurants.find({ name: { $eq: 'Wild Asia' } })
# 等价于
db.restaurants.find({ name: 'Wild Asia' })

# $ne 会筛选出并不包含该字段的文档，因为没有该字段也满足不等条件
db.restaurants.find({ cuisine: { $ne: 'Chinese' } })

db.newbooks.find({ rating: { $gt: 9 } })

# 按字母排序选出首字母 A 的
db.restaurants.find({ name: { $lt: 'B' } })

db.restaurants.find({ borough: { $in: ['Manhattan', 'Queens'] } })
# nin 同样会筛选出并不包含该字段的文档
db.restaurants.find({ borough: { $nin: ['Manhattan', 'Queens'] } })
```

#### 逻辑操作符

- $not - 匹配筛选条件不成立的文档 `{ <field>: { $not: { <expression> } } }`

- $and - 匹配多个筛选条件全部成立的文档 `{ $and: [<expression>, <expression>, ...] }`

- $or - 匹配至少一个筛选条件成立的文档 `{ $or: [<expression>, <expression>, ...] }`

- $nor - 匹配多个筛选条件全部不成立的文档 `{ $nor: [<expression>, <expression>, ...] }`

```bash
# 筛选出评分不小于 8 的结果，$not 同样会筛选出并不包含该字段的文档
db.newbooks.find({ rating: { $not: { $lt: 9 } } })
# 等价于
db.newbooks.find({ rating: { $gte: 9 } })

# $and 匹配不同字段：筛选出类型为 nonfiction 且评分大于等于 9 的结果
db.newbooks.find({
  $and: [
    { type: 'nonfiction' },
    { rating: { $gte: 9 } }
  ]
})
# 等价于
db.newbooks.find({
  type: 'nonfiction',
  rating: { $gte: 9 }
})
# $and 匹配相同字段：筛选出评分大于等于 8 小于 9 的结果
db.newbooks.find({
  $and: [
    { rating: { $gte: 8 } },
    { rating: { $lt: 9 } }
  ]
})
# 等价于
db.newbooks.find({ rating: { $gte: 8, $lt: 9 } })

# $or 操作符语法和 $and 一致，但没有等价的匹配查询语法
db.newbooks.find({
  $or: [
    { rating: { $gte: 9 } },
    { rating: { $lt: 1 } }
  ]
})
# $or 的条件使用 $eq 时等价于 $in
db.restaurants.find({
  $or: [
    { borough: { $eq: 'Manhattan' } },
    { borough: { $eq: 'Queens' } }
  ]
})
# 等价于
db.restaurants.find({ borough: { $in: ['Manhattan', 'Queens'] } })

# $nor 操作符语法和 $or 一致，同样会筛选出并不包含相应字段的文档
db.restaurants.find({
  $nor: [
    { borough: 'Manhattan' },
    { borough: 'Queens' }
  ]
})
# 以上等价于
db.restaurants.find({ borough: { $nin: ['Manhattan', 'Queens'] } })
```

#### 字段操作符

- $exists - 匹配包含查询字段的文档 `{ <field>: { $exists: <boolean> } }` [手册地址](https://docs.mongodb.com/manual/reference/operator/query/exists/)

- $type - 匹配字段类型复合查询值的文档 `{ <field>: { $type: <BSON type> or [<BSON type>, <BSON type>, ...] } }` [手册地址](https://docs.mongodb.com/manual/reference/operator/query/type/)

注：在 3.6 及以上版本 $type 才支持 array 参数

```bash
db.restaurants.find({ borough: { $exists: true } })
db.restaurants.find({ _id: { $type: 'string' } })
db.restaurant.find({ _id: { $type: ['objectId', 'object'] } })
# 一个比较常用的功能，查 null 类型
db.restaurant.find({ name: { $type: 'null' } })
```

也可以使用 BSON 类型序号查询，具体可以参考手册，但是可读性不好，不推荐。
