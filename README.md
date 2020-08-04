# 数据库

## 显示数据库

```
  show dbs
```

## 创建使用数据库

如果数据库存在，则选择数据库，如果不存在，则创建数据库**要添加数据后才会显示**

```
  use dbname
```

## 显示数据集合(表)

```
  show collections
```

## 删除数据库

```
db.dropDatabase()
```

## 删除集合

```
db.dbname.drop() => db.students.drop()
```

# 增删改查

## 增 insert

```
  db.dbname.insert({}) => db.students.insert({name:'zhangsan',age:24})
```

## 查 find

> pretty() 方法以格式化的方式来显示所有文档

```
{
    "_id" : ObjectId("5f28b0ac552842d899ce8d63"),
    "name" : "Yao",
    "age" : 35,
    "sex" : "male"
}
{
    "_id" : ObjectId("5f28b0bc552842d899ce8d64"),
    "name" : "Yi",
    "age" : 28,
    "sex" : "male"
}
```

1.  查询所有

```
  db.students.find()
```

2.  去掉重复数据

```
  db.students.distinct('name')
```

3.  查询具体条件

```
db.students.find({"age" : 22})
```

4.  查询 age > 22

```
db.students.find({age:{$gt:22}})
```

5.  查询 age < 22

```
db.students.find({age:{$lt:22}})
```

6.  查询 age >= 22

```
db.students.find({age:{$gte:22}})
```

7.  查询 age <= 25

```
db.students.find({age:{$lte;22}})
```

8. 查询 age >= 23 并且 age <= 26

```
db.students.find({age:{$gte:23,$lte:26}})
```

9. 模糊查询( name 中包含 mongo 的数据)

```
db.studetns.find({name:/mongo/})
```

10. 查询指定列

```
db.students.find({},{name:1,age:1})
// name可以要true或false。 false：显示name以为的信息m,true:相当于 name ： 1
```

11. 查询指定列 name 、age 数据， age > 25

```
db.students.find({age:{$gt:25}},{name:1,age:1})
```

12. 按照年龄排序查询

1:生序 -1:降序

```
db.students.find().sort({age:1})
db.students.find().sort({age:-1})
```

13. 查询 name = 'zhagnsan' age = 22

```
db.students.find({name:'zhagnsan',age:22})
```

14. 查询前 5 条数据

```
db.students.find().limit(5)
```

15. 查询 10 条以后的数据

```
db.students.find().skip(10)
```

16. 分页查询(第二页，每页 5 条,page:2,pagesize:5)

```
db.students.find().skip((page - 1) * pagesize).limit(pagesize)
db.students.find().skip(5).limit(5)
```

17. or 查询

```
db.students.find({$or:[{age:26},{age:31}]})
```

18. findOne 查询第一条数据

```
  db.students.findOne()
```

19. 统计数量

```
db.students.find({age:{$lt:27}}).count()
```

如果要返回限制之后的记录数据，要使用 count(true)或 count(false)

```
db.students.skit(10).limit(5).count(true)
```

## 改 update

```
db.collection.update(
	<query>,
	<update>,
	{
		upsert: <boolean>,
		multi: <boolean>,
		writeConcern: <document>
	}
)
```

> 参数说明

- query : update 的查询条件，类似 sql update 查询内 where 后面的。
- update : update 的对象和一些更新的操作符（如 $,$inc...）等，也可以理解为 sql update 查询内 set 后面的
- upsert : 可选，这个参数的意思是，如果不存在 update 的记录，是否插入 objNew,true 为插入，默认是 false，不插入。
- multi : 可选，mongodb 默认是 false,只更新找到的第一条记录，如果这个参数为 true,就把按条件查出来多条记录全部更新。
- writeConcern :可选，抛出异常的级别。

```
db.students.update({name:'zhagnsan'},{$set:{age:16}})
```

完整替换(不用使用 set 关键字)

```
db.students.update({name:'zhagnsan'},{name:'lisi',age:24,sex:'male'})
```

## 删除 remove

```
db.students.remove({id:1})
```

# 聚合

## 基本用法

> 集合数据

```
{
	_id: ObjectId(7df78ad8902c)
	title: 'MongoDB Overview',
	description: 'MongoDB is no sql database',
	by_user: 'w3cschool.cc',
	url: 'http://www.w3cschool.cc',
	tags: ['mongodb', 'database', 'NoSQL'],
	likes: 100
},
{
	_id: ObjectId(7df78ad8902d)
	title: 'NoSQL Overview',
	description: 'No sql database is very fast',
	by_user: 'w3cschool.cc',
	url: 'http://www.w3cschool.cc',
	tags: ['mongodb', 'database', 'NoSQL'],
	likes: 10
},
{
	_id: ObjectId(7df78ad8902e)
	title: 'Neo4j Overview',
	description: 'Neo4j is no sql database',
	by_user: 'Neo4j',
	url: 'http://www.neo4j.com',
	tags: ['neo4j', 'database', 'NoSQL'],
	likes: 750
}
```

| 表达式     | 描述                                           | 实例                                                                                  |
| ---------- | ---------------------------------------------- | ------------------------------------------------------------------------------------- |
| \$sum      | 计算总和。                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| \$avg      | 计算平均值                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| \$min      | 获取集合中所有文档对应值得最小值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| \$max      | 获取集合中所有文档对应值得最大值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| \$push     | 在结果文档中插入值到一个数组中。               | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])            |
| \$addToSet | 在结果文档中插入值到一个数组中，但不创建副本。 | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}])       |
| \$first    | 根据资源文档的排序获取第一个文档数据。         | db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}])    |
| \$last     | 根据资源文档的排序获取最后一个文档数据         | db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}])      |

## 管道

> 几种常见操作

- \$project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。

- $match：用于过滤数据，只输出符合条件的文档。$match 使用 MongoDB 的标准查询操作。
- \$limit：用来限制 MongoDB 聚合管道返回的文档数。
- \$skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
- \$unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
- \$group：将集合中的文档分组，可用于统计结果。
- \$sort：将输入文档排序后输出。
- \$geoNear：输出接近某一地理位置的有序文档。

1.  \$project 实例

```
db.article.aggregate(
	{
		$project : {
			title : 1 ,
			author : 1 ,
		}
	}
);
```

2. \$match 实例

```
db.articles.aggregate( [
	{ $match : { score : { $gt : 70, $lte : 90 } } },
	{ $group: { _id: null, count: { $sum: 1 } } }
] );
```

$match用于获取分数大于70小于或等于90记录，然后将符合条件的记录送到下一阶段 $group 管道操作符进行处理

# 数据备份

## 备份

在 MongoDB 安装目录的 bin 目录输入命令 mongodump

```
mongodump -h dbhost -d dbname -o dbdirectory
```

- -h

  MongDB 所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017

- -d

  需要备份的数据库实例，例如：test

- -o

  备份的数据存放位置，例如：c:\data\dump，当然该目录需要提前建立，在备份完成后，系统自动在 dump 目录下建立一个 test 目录，这个目录里面存放该数据库实例的备份数据

| 语法                                              | 描述                           | 实例                                             |
| ------------------------------------------------- | ------------------------------ | ------------------------------------------------ |
| mongodump --host HOST_NAME --port PORT_NUMBER     | 该命令将备份所有 MongoDB 数据  | mongodump --host w3cschool.cc --port 27017       |
| mongodump --dbpath DB_PATH --out BACKUP_DIRECTORY |                                | mongodump --dbpath /data/db/ --out /data/backup/ |
| mongodump --collection COLLECTION --db DB_NAME    | 该命令将备份指定数据库的集合。 | mongodump --collection mycol --db test           |

## 数据恢复

```
mongorestore -h dbhost -d dbname  dbdirectory
```

- -h：
  MongoDB 所在服务器地址

- -d：
  需要恢复的数据库实例，例如：test，当然这个名称也可以和备份时候的不一样，比如 test2

- --directoryperdb(备份时去掉--directoryperdb，不然会报错)：
  备份数据所在位置，例如：c:\data\dump\test，这里为什么要多加一个 test，而不是备份时候的 dump，读者自己查看提示吧！

- --drop：
  恢复的时候，先删除当前数据，然后恢复备份的数据。就是说，恢复后，备份后添加修改的数据都会被删除，慎用哦！
