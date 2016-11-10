## 使用 mongo Shell 删除数据

###  概览
使用`remove()`方法从`collection`中删除`document`。在此方法中指定一个条件以此来决定要删除的文档。

删除所需条件的结构和语法和查询的一样。不熟悉的可以看看[Find or Query Data with the mongo Shell](https://docs.mongodb.com/getting-started/shell/query/)

### 预备
本节的例子使用`test`数据库中名为`restaurants`的集合。关于向集合中填充样本数据的说明，请看[导入示例数据](https://docs.mongodb.com/getting-started/shell/import-data/)

在`mongo shell`中连接已运行的`mongod`实例，并切换到`test`数据库
```bash
use test
```
### 删除符合条件的所有文档
下面的操作将会删除符合指定条件的所有文档。
```bash
db.restaurants.remove( { "borough": "Manhattan" } )
```
删除操作返回一个包含操作状态的 `WriteResult` 对象。`nRemoved` 字段指定了被删除文档的数目。
Use the justOne Option
### 使用 `justOne` 选项
默认情况下，`remove()` 方法会删除符合条件的所有文档。使用 `justOne` 选项可以限制删除操作，使其仅仅删除一个符合删除条件的文档。
```bash
db.restaurants.remove( { "borough": "Queens" }, { justOne: true } )
```
成功的操作应返回像下面这样的 [WriteResult](https://docs.mongodb.com/manual/reference/method/WriteResult/#WriteResult) 对象！
```
WriteResult({ "nRemoved" : 1 })
```
在这种情况下，`nRemoved` 字段指定被删除的文档数目。
### 删除所有数据

要想删除集合中所有文档，只需传递给 `remove()` 方法一个空的文档`{}`作为条件即可。
```bash
db.restaurants.remove( { } )
```

此删除操作返回一个包含操作状态的 WriteResult 对象。nRemoved 字段指定了被删除文档的数目。 

### 删除集合

上面的操作仅仅删除了集合中的全部文档。集合本身和关于集合的索引依然存在。相对于删除集合中的所有文档来说，更有效率的也许是删除整个集合以及索引后重建集合和索引。
```bash
db.restaurants.drop()
```
上面的操作成功后将会返回`true`。
```bash
true
```
如果要删除的集合不存在，此操作将会返回`false`。

### 附加信息

在 `MongoDB` 手册中，查看 `remove()` 和 `drop()`
`MongoDB` 中，单个文档上的 `write` 是原子操作。如果单个删除操作要删除集合中的多个文档，那么此操作可以和其他 `write` 操作一起使用。在 `MongoDB` 手册中查看 [Atomicity](http://docs.mongodb.com/manual/core/write-operations-atomicity)
