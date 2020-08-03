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

## 增

```
  db.dbname.insert({}) => db.students.insert({name:'zhangsan',age:24})
```

## 查 find

1.  去掉重复数据

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
