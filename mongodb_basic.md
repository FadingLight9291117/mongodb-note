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