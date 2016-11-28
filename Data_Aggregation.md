## 数据聚合
MongoDB 可以执行聚合操作，例如通过指定的键进行分组并评估每个不同组的总数或计数。
使用 `aggregate()` 方法执行基于阶段的聚合。 `aggregate()` 方法接受一个阶段数组(列表)作为参数，每个阶段按顺序执行。
```bash
db.collection.aggregate( [ <stage1>, <stage2>, ... ] )
```
#### 预备

本节的例子使用test数据库中名为restaurants的集合。关于向集合中填充样本数据的说明，请看导入示例数据

在mongo shell中连接已运行的mongod实例，并切换到test数据库
```bash
use test
```
#### 通过一个字段对文档分组并计
使用 `$group` 阶段并通过指定的键分组。在 ` ` 阶段，使用 `_id` 字段的的键指定分组。
```bash
db.restaurants.aggregate(
   [
     { $group: { "_id": "$borough", "count": { $sum: 1 } } }
   ]
);
```
结果：
```bash
{ "_id" : "Staten Island", "count" : 969 }
{ "_id" : "Brooklyn", "count" : 6086 }
{ "_id" : "Manhattan", "count" : 10259 }
{ "_id" : "Queens", "count" : 5656 }
{ "_id" : "Bronx", "count" : 2338 }
{ "_id" : "Missing", "count" : 51 }
```
`_id` 字段包含不同的 `borough` 值。

#### 过滤并分组文段
使用 `$match` 阶段过滤文档。`$match` 使用 `MongoDB` 的查询语法。下面的操作使用 `$match` 查询 `restaurants` 集合中 `borough` 等于`Queens`和 `cuisine` 等于 `Brazilian` 的文档。然后 `$group` 阶段通过 `address.zipcode` 字段对匹配的文档进行分组并使用 `$sum`  累加器计算数目。`$group` 通过字段路径访问字段，字段名前加一个`$`.
```bash
db.restaurants.aggregate(
   [
     { $match: { "borough": "Queens", "cuisine": "Brazilian" } },
     { $group: { "_id": "$address.zipcode" , "count": { $sum: 1 } } }
   ]
);
```
结果：
```bash
{ "_id" : "11368", "count" : 1 }
{ "_id" : "11106", "count" : 3 }
{ "_id" : "11377", "count" : 1 }
{ "_id" : "11103", "count" : 1 }
{ "_id" : "11101", "count" : 2 }
```

`_id` 字段包含不同的 `zipcode` 值。 
Additional Information
#### 附加信息
在 MongoDB 手册中，查看 `aggregate()`
