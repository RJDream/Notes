# MongoDB



## Basic

| Method   | Means                        |
| -------- | :--------------------------- |
| show dbs | display all db that in mongo |
|db.createCollection("dbname",{capped:true,size:2014});|创建一个固定大小的集合|
|||
|||
|||
|||







## Data Type

| Type   | Means |
| ------ | ----- |
| String | 字符串，在mongo中utf-8编码的才是合法的 |
|Integer|整数，根据服务器分为32/64位|
|Boolean|布尔值，真/假|
|Double|双精度浮点数|
|Min/Max keys|将一个值与BSON（二进制JSON）元素中的最低值和最高值比较|
|Array|将多个值存储到一个键|
|Timestamp|时间戳，记录文档添加/修改的具体时间|
|Object|内嵌文档|
|Null|空值|
|Symbol|符号，基本等同与String，不同的是它一般用于采用特殊符号类型的语言|
|Date|日期时间，unix时间格式存储当前日期或时间|
|Object ID|对象id，用于创建文档的id|
|Binary Data|二进制数据|
|Code|代码类型，用于在文档中存储JS代码|
|Regular expression|正则表达式|



***Object ID***

类似唯一主键，可以很快生成和排序，包含12 bytes

0-3 bytes：unix时间戳

4-6 bytes：机器标识码

7-8 bytes：进程id组成的pid

9-11 bytes：随机数

**文档中必须有一个_id键，可以是任意类型的，默认是ObjectID类型**





***String***

**BSON**字符串都是**UTF-8**编码





## 创建数据库

**use db_name**

如果数据库存在，切换到数据库，否则创建该数据库



新创建的数据库不会出现在 **show dbs** 结果中，必须插入数据后才会显示

db.collection_name.insert({key:val,key:val ...});

与创建数据库的逻辑一样，如果collection_name 不存在就创建。

**show tables** / **show collections**可以查看有哪些collection





## 删除数据库

**db.dropDatabase()**

删除当前数据库，默认为test， 可以使用**db** 查看当前数据库



## 创建集合

**db.createCollection(name,options)**

name: 指定集合名称

options: 可选参数

| Field       | Type    | desc                                                         |
| ----------- | ------- | ------------------------------------------------------------ |
| capped      | boolean | 是否创建固定大小集合，该类型集合会自动覆盖最早的数据，如果为true，必须指定size |
| autoIndexId | boolean | 是否自动在_id 字段创建索引，default：false                   |
| size        | number  | 指定集合的最大字节数                                         |
| max         | number  | 指定固定集合中包含文档的最大数量                             |

在插入文档时，会先检查固定集合的size，然后检查max





## 删除集合

**db.collection_name.drop()**





## 插入文档

文档的数据结构和json类似，所有存储在mongo中的数据都是BSON，是一种类json的二进制形式存储格式，简称Binary JSON



### insert

**db.collection_name.insert(doc)**

```js
var obj = {
	"title":"book", 
	desc:"test insert",
	by:"rj",
    url:"http:12y.0.0.1",
    tags:["mongodb","database","collection","document"],
    likes:100    
	};

db.col.insert(obj);
```



### save

**db.collection_name.save(doc)**

save如果不指定_id字段的时候和 insert 类似，如果指定了_ _id那么则会更新该字段







## 更新文档

### update

```js
db.collection.update(
	<query>,
    <udpate>,
    {
    	upsert:<boolean>,
    	multi:<boolean>
    	writeConcern:<document>
    }
)


/**
 * query: update 的查询条件
 * update：update的对象和一些更新的操作符，
 * upsert：可选，如果需要更新的对象不存在，是否插入该对象
 * multi：default=false， 默认只更新第一条找到的数据，若为true，更新所有找到的数据
 * writeConcern:可选，抛出异常级别
 */

```

**insert data**

```js
db.student.insert({name:'zhangsan',age:15,cls:'1-1'})
db.student.insert({name:'lisi',age:16,cls:'1-2'})
db.student.insert({name:'wangwu',age:16,cls:'1-1'})
db.student.insert({name:'zhaoliu',age:15,cls:'1-1'})

```



```js
//update first docuemnt
db.student.update({age:16},{$set:{cls:'2-1'}});

//update all document
db.student.update({age:15},{$set:{cls:'1-2'}},{multi:true});
```





### save 方法

```js
db.collection.save(
	<document>,
    {
    	writeConcern:<document>
    }
);

/**
 * save 方法是替换文档，相当于先删除原先的文档，再插入新的文档
 * document: 文档数据
 * writeConcern: 可选，抛出异常级别
 */
```



```js
db.student.save({
    "_id" : ObjectId("5af9462dba32fde9fdd3c066"),
    name:'peter'
})

//该_id的文档现在只有一个name key
```



## 删除文档

```js
db.collection.remove(<query>,<jsutOne>)

/**
 * 
 *
 */
```





## 查询文档	

```js
db.collection.find(query,projection);


/**
 * query 可选，使用查询操作符指定查询条件
 * projection 可选，使用投影操作符指定返回的键，默认返回所有键
 * 使用pretty() 格式化返回结果
 */
```





| 操作     | 格式                   | 范例                             | RDBMS           |
| -------- | ---------------------- | -------------------------------- | --------------- |
| 等于     | {<key>:<value>}        | db.student.find({age:16})        | where age = 16  |
| 小于     | {<key>:{$lt:<value>}}  | db.student.find({age:{$lt:16}})  | where age < 16  |
| 小于等于 | {<key>:{$lte:<vlaue>}} | db.student.find({age:{$lte:15}}) | where age <= 15 |
| 大于     | {<key>:{$gt:<value>}}  | db.student.find({age:{$gt:15}})  | where age > 15  |
| 大于等于 | {<key>:{$gte:<value>}} | db.student.find({age:{$gte:16}}) | where age >= 16 |
| 不等于   | {<key>:{$ne:<value>}}  | db.student.find({age:{$ne:15}}); | where age <> 15 |



**$ne**

不等于会将集合中不存在该键的文档也查询出来

### and

将query中的多个查询使用逗号分隔，就是and

```js
db.student.find({age:{$lt:16}, cls:'1-1'});
```



### or

mongo 使用关键字$or ， 将多个query放在一个数组中

```js
db.student.find({
    $or:[
        {age:{$gt:15}}, {cls:'1-1'}
    ]
})
```



```shell
> db.student.find({     $or:[         {age:{$gt:15}}, {cls:'1-1'}     ] }).pretty()
{
        "_id" : ObjectId("5af9462aba32fde9fdd3c064"),
        "name" : "lisi",
        "age" : 16,
        "cls" : "2-1"
}
{
        "_id" : ObjectId("5af9462aba32fde9fdd3c065"),
        "name" : "wangwu",
        "age" : 16,
        "cls" : "1-1"
}
```



### combine and / or

```js
db.student.find({age:16}, $or:[{cls:'1-1'}]);


db.student.find({$or:[{age:15,cls:'1-1'}, {age:16, cls:'1-2'}]});
```



### projection

```js
// 指定返回的键
db.student.find({},{name:1});
//error start ----------------------------------------
//error , 指定指定返回键或返回的键，不能同时指定那些键返回，那些键不返回
db.student.find({},{name:1, cls:0});
Error: error: {
        "ok" : 0,
        "errmsg" : "Projection cannot have a mix of inclusion and exclusion.",
        "code" : 2,
        "codeName" : "BadValue"
}
// error end ----------------------------------------
db.student.find({},{cls:0});

```



## 操作符

- $lt	  小于
- $lte:  小于等于
- $gt    大于
- $gte 大于等于
	 $ne	 不等于
- $eq   等于
- $type  基于BSON类型来检索集合中匹配的数据类型，并返回结果

```js
//如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：
db.col.find({"title" : {$type : 2}})

```



| **类型**                | **数字** | **备注**         |
| ----------------------- | -------- | ---------------- |
| Double                  | 1        |                  |
| String                  | 2        |                  |
| Object                  | 3        |                  |
| Array                   | 4        |                  |
| Binary data             | 5        |                  |
| Undefined               | 6        | 已废弃。         |
| Object id               | 7        |                  |
| Boolean                 | 8        |                  |
| Date                    | 9        |                  |
| Null                    | 10       |                  |
| Regular Expression      | 11       |                  |
| JavaScript              | 13       |                  |
| Symbol                  | 14       |                  |
| JavaScript (with scope) | 15       |                  |
| 32-bit integer          | 16       |                  |
| Timestamp               | 17       |                  |
| 64-bit integer          | 18       |                  |
| Min key                 | 255      | Query with `-1`. |
| Max key                 | 127      |                  |





## Limit & Skip

```js
//读取指定条数的数据
db.collection.find().limit(number);

//跳过指定数量的文档
db.collection.find().skip(number);
```



## Sort

```JS
db.collecton.find().sort({key:1})

// 1 as asc
// -1 as desc
```





## Index

索引是特殊的数据结构，存储在一个易于遍历读取数据的数据集合中，索引是对数据库集合中的一列或多列值进行排序的一种结构。



### 创建索引

```js
//按升序创建索引
db.collection.ensureIndex({key:1});
//按降序创建索引
db.collection.ensureIndex({key:-1});
```

ensureIndex() 接收可选参数，可选参数列表如下： 

| Parameter          | Type          | Description                                                  |
| ------------------ | ------------- | ------------------------------------------------------------ |
| background         | Boolean       | 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background"     可选参数。  "background" 默认值为**false**。 |
| unique             | Boolean       | 建立的索引是否唯一。指定为true创建唯一索引。默认值为**false**. |
| name               | string        | 索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。 |
| dropDups           | Boolean       | 在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 **false**. |
| sparse             | Boolean       | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 **false**. |
| expireAfterSeconds | integer       | 指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。 |
| v                  | index version | 索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。 |
| weights            | document      | 索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。 |
| default_language   | string        | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语 |
| language_override  | string        | 对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 language. |



## 聚合

```js
db.collection.aggregate(aggregate_operation);
```



```js
//计算年龄之和
db.student.aggregate([{$group:{_id:"$cls", num_tutorial:{$sum:1}}}]);
```

$match 