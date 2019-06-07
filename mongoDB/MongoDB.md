# MongoDB



# 介绍





# 安装

mongoDB下载地址：https://www.mongodb.com/download-center#community

客户端工具的下载地址：https://studio3t.com/



解压mongoDB

配置环境变量

创建数据存储地址

启动mongoDB：mongod --dbpath=[datapath]

mongoDB默认端口号：27017



将mongoDB安装位windows服务

```shell
db ##当前所在的数据库,默认连接名称为“test”的数据库
```



mongoDB的常用命令

```javascript
use #没有数据库则创建数据库，并且存于内存中，如果有数据库则切换到对应的数据库
show dbs ##查看所有的数据库
use mydb1 #创建数据库mydb1
show collections #显示当前数据库的所有集合
db.c1.insert({name:"jack",age:18}) ##常见集合并向集合中添加数据“{name:"jack",age:18}”
db.dropDatabase() ##删除数据库
db.help() ##查看db后追加的所有命令
db.createCollection("[集合名称]") ##显式创建集合
db.[集合名称].drop() ##删除集合
db.[集合名称].count() ##统计集合记录数
db.[集合名称].remove() ##删除集合中所有的document
```



向mongDB中插入10000条记录

```
for(var i = 0; i < 10000; i++){
    db.person.insert({name:"user" + i,age:i})
}
```



按照条件删除moongDB中的document

```javascript
db.[集合名称].remove({key:value}) ##按照条件删除数据
```



查询集合中的文档

```javascript
db.[集合名称].find(查询条件)
db.[集合名称].findOne(查询条件) ##查询第一条记录

##只查部分字段
db.[集合名称].find(查询条件,查询字段)
db.user.find({name:""},{name:1}) ##1表示true显示，0表示false不显示

##使用条件表达式
//大于
db.collection.find({field:{$gt:value}})
//小于
db.collection.find({field:{$lt:value}})
//大于等于
db.collection.find({field:{$gte:value}})
//小于等于
db.collection.find({field:{$lte:value}})
//不等于
db.collection.find({field:{$ne:value}})

##查询集合中的文档，统计count、排序sort、分页skip、limit
db.collection.count()
db.collection.find().count()
db.collection.find({age:{$lt:5}}).count()

```









