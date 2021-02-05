# :computer:MongoDB Basics课程笔记



## :house:关于mongodb

### :point_right:MongoDB的结构

![image-20210204144951071](imgs\mongodb_database_new.png)

### :point_right:MongoDB Atlas

> MongoDB的云服务产品
>
> https://cloud.mongodb.com



## :house:增删改查数据

### :point_right:存储数据

**JSON** vs **BSON**

> `BSON`, 桥接了二进制表示法和JSON的格式 
>
> MongoDB stores data in `BSON` , and you can then view it in `JSON`. `BSON` is faster to parse and lighter to store than `JSON`. `JSON` supports fewer data types than `BSON`.

关于什么是`BSON`，可以看下面两个网站

https://www.mongodb.com/json-and-bson

http://bsonspec.org/

### :point_right:导入和导出数据

| JSON        | BSON         |
| ----------- | ------------ |
| mongoimport | mongorestore |
| mongoexport | mongodump    |



---

:exclamation: ​注意下面的导出导出都是操作MongoDB Altas的

---

#### 导出

`BSON`导出

```shell
mongodump --uri "<Altas Cluster URI>"
```

`JSON`导出

```shell
mongoexport --uri "<Altas Cluseter URI>"
			--collection=<collection name>
			--out=<filename>.json
```



#### URI String

> Uniform Resource Identifier: 统一资源标识符
>
> URL -> Uniform Resource Locator: 统一资源定位符

```shell
mongodb+srv://<username>:<password>@<cluster>.mongodb.net/<databaseName>
```

- `srv` - establishes a `secure` connection

比如

```shell
mongodump --uri "mongodb+srv://clz:m4a1421@sandbox.0oosp.mongodb.net/sample_supplies"
```

```shell
mongoexport --uri "mongodb+srv://clz:m4a1421@sandbox.0oosp.mongodb.net/sample_supplies"
			--collection=sales
			--out=sales.json
```



#### 导入

导入`BSON`

```python
mongorestore --uri "mongodb+srv://clz:m4a1421@sandbox.0oosp.mongodb.net/sample_supplies"
			--drop dump
```

导入`json`

```shell
mongoimport --uri "mongodb+srv://clz:m4a1421@sandbox.0oosp.mongodb.net/sample_supplies"
			--drop sales.json
```



### :point_right:数据查询

:point_down:在MongoDB Atlas的web控制台查询文档

![image-20210204194044710](F:\learning_mongodb\imgs\altasGUI.png)

### :key:find命令

#### 命令行连接MongoDB Atlas

```shell
mongo "mongodb+srv://clz:m4a1421@sandbox.0oosp.mongodb.net/admin" # URI
```

`admin` - target authentication database name(目标认证数据库名称), 就是数据库的名称，不存在也没关系。

连接上之后的一些mongo命令:point_down:

```js
show dbs

use sample_training

show collections

db.zips.find({"state": "NY"})

db.zips.find({"state": "NY"}).count()						// 计算返回的数量

db.zips.find({"state": "NY", "city": "ALBANY"})

db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()	// 美化输出
```

---

:mag:提示

![image-20210204204103854](\imgs\it.png)

这个`it`意思是`iterator`

---

#### :key:总结

- `show dbs`和`show collections`
- `find()`用来返回一个cursor和匹配的查询结果
- `count()`返回查询的数量
- `pretty()`格式化输出文档

## :house:创建和操作文档

### :point_right:插入新的文档

在mongodb atlas中:point_down:

![image-20210205162702074](F:\learning_mongodb\imgs\insertIntoAtlas.png)



:key:`_id` - 每个文档都必须有一个唯一的`_id`值

#### ObjectId()

> `_id`字段的默认值，除非指定

Example:

```json
"_id": ObjectId("5e222d45a5f2f457b")

"_id": "710ca992"

"_id": "101-EXG-27"
```



#### :key:Insert() and errors

```shell
db.inspections.findOne();	# 获取一个随机的文档
```

```shell
db.inspections.insert({"_id": ObjectId(), "id": "123"}) # 插入文档
```

:key:如果插入的文档没有`_id`字段，mongdb会在插入文档前自动添加`_id`字段。



#### Order

```shell
db.inspection.insert(
[{"_id": 1, "test": 1}, {"_id": 2, "test": 2}, {"_id": 3, "test": 3}],
{"ordered": false}
)
```

使用`{"ordered": false}`来取消默认的插入顺序。

> - `ordered` 默认为`true`，即顺序写入。设置为`false`的时候，表示乱序写入，可以提高操作性能。
> - 假如批量插入多条数据的话，`ordered` 为 `true`，则插入过程中报错的话，后面的插入就会中断
> - 假如批量插入多条数据的话，`ordered` 为`false`，则插入过程中报错的话，后面的插入会照常执行
>
> ——CSDN

---

:mag:`database`和`collection`会被自动创建，当使用`use`, 并且`insert`时​



### :point_right:更新文档

#### 在Atlas中:point_down:

![image-20210205190517714](imgs\updateIntoAltas.png)



#### mongo shell

> Updating Documents: **MQL**

:point_down:

一些函数

| One         | Many         |
| ----------- | ------------ |
| updateOne() | updateMany() |
| findOne()   | find()       |



#### :key:更新操作

```json
{"$inc": {"pop": 10, "<field2>": "<increment value>", ...}}		// 字段的值增加指定的数字
 
{"$set": {"pop": 17630, "<field2>": "<increment value>", ...}}	// 设置字段的值

{"$push": {<field1>: <value1>, ...}}							// 添加一个元素到数组字段
```



例如

set

```json
db.zips.updateOne({"zip": "12534"}, {"$set": {"pop": 17630}})
```

push

```json
db.grades.updateOne({"student_id": 250, "class_id": 399}, 
                    {"$push": {"scores": {"type": "extra credit",
                                          "score": 100}
                              }
                    })
```



### :point_right:删除文档和集合

- deleteOne()
- deleteMany()

:key:删除collection使用`drop()`

```shell
db.inspection.drop()
```



## 高级CRUD