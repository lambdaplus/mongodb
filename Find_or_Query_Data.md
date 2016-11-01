## [使用 mongo shell 查找或查询数据](https://docs.mongodb.com/getting-started/shell/query/#find-or-query-data-with-the-mongo-shell)

### 概述

可以使用 `find()` 方法来从 MongoDB 的集合中查询数据.所有的查询结果都限于 MongoDB 中的单个集合。

查询可以返回集合中的所有文档,或者仅仅返回特定(通过条件过滤)的文档.需要把条件作为 `find()` 的参数。

`find()` 方法返回的是一个游标(cursor),它是一个可返回文档的可迭代对象。

### 预备

本节的例子将使用 `test` 数据库中名为 `restaurants` 的集合。关于样本数据集中集合的填充说明，请参阅 [Import Example Dataset](https://docs.mongodb.com/getting-started/shell/import-data/)

在 mongo shell 中连接一个运行的 mongod 实例, 切换到 `test` 数据库。
```bash
> use test
```

### 查询集合中的所有文档

如果要返回集合中的所有文档,就不要在调用 `find()` 方法时使用任何条件作为参数.举个栗子, 下面这个操作就会返回 `restaurants` 集合中的所有文档。

```bash
> db.restaurants.find()

```

### 添加特定条件

查询的条件遵从如下形式:
`{ <field1>: <value1>, <field2>: <value2>, ... }`

有两点需要注意:

- 如果<field>是顶层字段并且不是嵌套的文档或者数组,字段(field)的名字可以不使用引号。


- 如果 <field> 是嵌套的文档或数组,可以使用圆点符号(.)访问字段。使用圆点符号时，必须使用引号括起来。

### 通过顶层字段查询

下面的操作将会找到 `borough` 字段名等于“Manhattan”的文档。
```bash
> db.restaurants.find( { "borough": "Manhattan" } )

> db.restaurants.find({borough:'Manhattan'}) # 前面说到 顶层字段可以不用引号，事实证明 mongo 是没有骗人的，哈哈！

```

返回的集合中只会包含匹配的文档。

#### 查询嵌入文档中的字段

使用 圆点符号 为嵌入文档中的字段指定条件时，圆点符号 要求整个字段名都要用引号括起来。下面的操作演示了为嵌入 `address` 文档中的 `zipcode` 字段指定查询条件。
```bash
> db.restaurants.find( { "address.zipcode": "10075" } )
```
关于更多在嵌入文档中查询字段的方法信息，请看 [Query on Embedded Documents](https://docs.mongodb.com/manual/tutorial/query-documents/#read-operations-subdocuments)

#### 在数组中查询字段

`grades` 数组中包含嵌入的文档，使用圆点符号，并指定一个条件来进行查询。下面的例子演示了 从 `grades` 数组中找出嵌入的文档里 `grade` 等于 B的字段。
```bash
> db.restaurants.find( { "grades.grade": "B" } )

```

更多关于在数组中查询信息的方法， 比如指定多个条件 请参阅[Query on Arrays](https://docs.mongodb.com/manual/tutorial/query-documents/#read-operations-arrays) 和 $elemMatch

### 指定带运算符的条件

MongoDB 提供运算符来指定查询条件，例如 比较(comparsion)运算符。尽管有些例外， 比如 `$or`和`$and`条件运算符，但是使用运算符的条件查询大都符合如下形式。
`{ <field1>: { <operator1>: <value1> } }`

完整的运算符列表，请查阅[query operators](http://docs.mongodb.com/manual/reference/operator/query)

#### 大于运算符(`$gt`)

查询嵌入 `grades` 数组中字段分数大于30的文档
```
db.restaurants.find( { "grades.score": { $gt: 30 } } )

```


#### 小于运算符(`$lt`)

查询嵌入 `grades` 数组中字段分数小于10的文档
```bash
db.restaurants.find( { "grades.score": { $lt: 10 } } )
```

### 组合条件

可以通过使用逻辑与(AND)和逻辑或(OR)把多个查询条件组合在一起使用 

#### 逻辑与

同时使用两个查询条件，并用逗号把她们隔开。多个条件也不是不可以，记得用逗号隔开就行。当所有条件都符合时才能正确查询。
```bash
> db.restaurants.find( { "cuisine": "Italian", "address.zipcode": "10075" } )
```

#### 逻辑或

使用逻辑或组个多个条件，符合其中一个条件就行。
```bash
db.restaurants.find(
   { $or: [ { "cuisine": "Italian" }, { "address.zipcode": "10075" } ] }
)
```

### 对查询结果排序

对结果集指定顺序，可以在查询时添加`sort()`方法。并在`sort()`方法中添加包含`field(s)`的文档， 以此来确定排序方式以及相应的排序类型。

举个栗子：下面的操作返回`restaurants`集合中所有的文档，文档首先以`borough`字段的值升序排列， 然后， 对于每一个`borough`又以`address.zipcode`字段的值升序排列。
```bash
db.restaurants.find().sort( { "borough": 1, "address.zipcode": 1 } )
```
这个操作将会返回指定顺序的排序结果。
