## 使用 mongo shell 更新数据

### 概览

可以使用 `update()`方法更新集合中的文档。此方法接受如下形式的参数

- 一个过滤器文档用来匹配想要更新的文档
- 一个指定了修改的更新文档
- 一个选项参数(可选)

过滤器的结构和语法和查询用的一样。[Find or Query Data with the mongo Shell](https://docs.mongodb.com/getting-started/shell/query/) 是对查询条件的简单介绍。
.
默认情况下，`update()`方法只会更新单个文档，使用多个选项可以更新符合条件的全部文档。

你不能更新 `_id` 字段

### 预备

本节中的例子使用的是 `test`数据库中的 `restaurants`集合。有关样本数据集中集合填充的说明，请参阅 [Import Example Dataset](https://docs.mongodb.com/getting-started/shell/import-data/)

在 `mongo shell`中连接一个运行的 `mongod` 实例。并切换到 `test` 数据库。
```bash
> use test
```

### 更新指定的字段

为了改变字段的值，`MongodB` 提供了更新操作，比如使用 `$set` 修改字段值。对于一些更新操作，比如 `$set` 会创建一个不存在的字段。了解更多关于更新操作的方法请查阅 [update operators](http://docs.mongodb.com/manual/reference/operator/update)

#### 更新顶层字段

下面的操作将会更新名为 `Juni` 的第一个文档。使用 `$set` 更新 `cuisine` 字段，`$currentDate` 将`lastModified` 字段的值更新为当前日期(最后修改日期)。
```bash
db.restaurants.update(
    { "name" : "Juni" },
    {
      $set: { "cuisine": "American (New)" },
      $currentDate: { "lastModified": true }
    }
)
```

更新操作返回一个 `WriteResult` 对象，其包含了操作的状态。

#### 更新嵌套的字段

更新嵌入文档中的字段，要使用圆点符号。当然了，别忘了对字段名使用引号。下面的操作会更新嵌入 `address`文档中的 `street` 字段。
```bash
db.restaurants.update(
  { "restaurant_id" : "41156888" },
  { $set: { "address.street": "East 31st Street" } }
)
```

更新操作返回一个 `WriteResult` 对象，其包含了操作的状态。

#### 更新多个文档

`update()` 方法默认只会更新单个文档。使用多个选项可以更新多个文档。下面的操作会更新所有 `address.zipcode`字段值为`10086` 的文档，把 `cuisine` 字段的值设置为 `Category To Be Determined`并且把 `lastModified` 设置为当前日期。
```python
db.restaurants.update(
  { "address.zipcode": "10016", cuisine: "Other" },
  {
    $set: { cuisine: "Category To Be Determined" },
    $currentDate: { "lastModified": true }
  },
  { multi: true}
)
```


更新操作返回一个 `WriteResult` 对象，其包含了操作的状态。

### 更换(replace)文档

更新除 `_id ` 字段外的全部文档，只需把整个新文档作为 `update()` 的第二个参数就行了。作为替代的文档可以忽略 `_id` 字段， 因为它是不可变的。如果你确实包含了 `_id` 字段，那么它的值必须和原来的一样。
> 重点
>更新之后，文档仅仅包含此取代文档内的字段。**翻译一下： 原来被取代的文档啥也没有了， 只会留下新文档的内容。当然喽，`_id` 字段肯定会留下来的**

下面的文档在更新之后，被修改的文档只有`_id`、`name`、`address` 字段留下来。`restaurants_id` `cuisine` `grade` 和 `borough` 字段都不存在了。
```bash
db.restaurants.update(
   { "restaurant_id" : "41704620" },
   {
     "name" : "Vella 2",
     "address" : {
              "coord" : [ -73.9557413, 40.7720266 ],
              "building" : "1480",
              "street" : "2 Avenue",
              "zipcode" : "10075"
     }
   }
)
```

更新操作返回一个 `WriteResult` 对象，其包含了操作的状态。

### 附加信息

如果更新条件没有配到任何文档，其默认行为是什么都不做。把 `upsert` 选项设为 `True`，当匹配到文档时就更新，否则就作为新文档直接插入。更多信息请参阅 `MongoDB Manual` 的 `update()`相关章节。

在 `MongoDB` 中，单个文档的 `write` 操作在是原子(atomic)水平的。如果单个更新操作修改了集合中的多个文档，那么在此集合中，这个操作可以和其他 `write` 操作交错使用。更多信息请参阅 `MongoDB Manual` 的 `Atomicity`相关章节。

关于更多 `update` 的操作方法，请查看 `MongoDB Manual` 中 [Update Operators](http://docs.mongodb.com/manual/reference/operator/update) 章节。
