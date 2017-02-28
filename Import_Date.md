
## [导入示例数据库](https://docs.mongodb.com/getting-started/shell/import-data/#import-example-dataset)


### 概述
这个向导的例子使用的 `test`数据库中的 `restaurants` 集合。下面是一些示例文档。
```
{
  "address": {
     "building": "1007",
     "coord": [ -73.856077, 40.848447 ],
     "street": "Morris Park Ave",
     "zipcode": "10462"
  },
  "borough": "Bronx",
  "cuisine": "Bakery",
  "grades": [
     { "date": { "$date": 1393804800000 }, "grade": "A", "score": 2 },
     { "date": { "$date": 1378857600000 }, "grade": "A", "score": 6 },
     { "date": { "$date": 1358985600000 }, "grade": "A", "score": 10 },
     { "date": { "$date": 1322006400000 }, "grade": "A", "score": 9 },
     { "date": { "$date": 1299715200000 }, "grade": "B", "score": 14 }
  ],
  "name": "Morris Park Bake Shop",
  "restaurant_id": "30075445"
}
```
### 首先
必须启动 mongod 实例，以便导入数据。

1. 检索 `restaurants` 数据
从https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/primer-dataset.json中检索数据集并保存为`primer-dataset.json`。

2. 向集合导入数据
在系统 shell 或 命令行 执行 `mongoimport` 指令，把 `restaurants` 集合中的文档插入到 test 数据库。如果 `test` 数据库中已经存在此集合，那么此操作首先会清除
掉 `restaurants` 集合。
```
mongoimport --db test --collection restaurants --drop --file ~/downloads/primer-dataset.json
```
`mongoimport` 连接到运行在本地服务器上端口号为 27017 的 `mongod` 实例。--file 选项提供导入数据的路径。本例中是

    ～/downloads/primer-dataset.json
当然也可以在 `mongoimport` 命令中 自行指定 主机名 和 端口号。在 --host 和 --port后面添加即可。
