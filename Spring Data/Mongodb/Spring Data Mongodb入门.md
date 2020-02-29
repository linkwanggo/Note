

# Spring Data Mongodb入门

[spring data系列demo演示](https://github.com/spring-projects/spring-data-book)

## 目标

- 理解MongoDb的特点和体系结构 
- 掌握常用的MongoDB命令 
- 能够运用Java操作MongoDB 
- 使用SpringDataMongoDB完成微服务的开发 

## 简介

MongoDB 是一个跨平台的，面向文档的数据库，是当前 NoSQL 数据库产品中最热 

门的一种。它介于关系数据库和非关系数据库之间，是非关系数据库当中功能最丰富，最 

像关系数据库的产品。它支持的数据结构非常松散，是类似 JSON 的 BSON 格式，因此可以 

存储比较复杂的数据类型。 

MongoDB 的官方网站地址是：http://www.mongodb.org/ 

## 适用类型

- 数据量大 
- 写入操作频繁 
- 价值较低 

对于这样的数据，更适合使用MongoDB来实现数据的存储 

## 特点

- （1）面向集合存储，易于存储对象类型的数据 
- （2）模式自由 
- （3）支持动态查询 
- （4）支持完全索引，包含内部对象 
- （5）支持复制和故障恢复 
- （6）使用高效的二进制数据存储，包括大型对象（如视频等） 
- （7）自动处理碎片，以支持云计算层次的扩展性 
- （8）支持 Python，PHP，Ruby，Java，C，C#，Javascript，Perl 及 C++语言的驱动程 
- 序，
- 社区中也提供了对 Erlang 及.NET 等平台的驱动程序 
- （9） 文件存储格式为 BSON（一种 JSON 的扩展） 

## 基本体系结构

![](https://tva1.sinaimg.cn/large/006tNbRwly1gb10ow7rykj30oe0fnjsn.jpg)

## 数据类型

基本数据类型 

- null：用于表示空值或者不存在的字段，{“x”:null} 
- 布尔型：布尔类型有两个值true和false，{“x”:true} 
- 数值：shell默认使用64为浮点型数值。{“x”：3.14}或{“x”：3}。对于整型值，可以使用 NumberInt（4字节符号整数）或NumberLong（8字节符号整数）， {“x”:NumberInt(“3”)}{“x”:NumberLong(“3”)} 
- 字符串：UTF-8字符串都可以表示为字符串类型的数据，{“x”：“呵呵”} 
- 日期：日期被存储为自新纪元依赖经过的毫秒数，不存储时区，{“x”:new Date()} 

- 正则表达式：查询时，使用正则表达式作为限定条件，语法与JavaScript的正则表达式相 
- 同，{“x”:/[abc]/} 
- 数组：数据列表或数据集可以表示为数组，{“x”： [“a“，“b”,”c”]} 
- 内嵌文档：文档可以嵌套其他文档，被嵌套的文档作为值来处理，{“x”:{“y”:3 }} 
- 对象Id：对象id是一个12字节的字符串，是文档的唯一标识，{“x”: objectId() } 
- 二进制数据：二进制数据是一个任意字节的字符串。它不能直接在shell中使用。如果要 
- 将非utf-字符保存到数据库中，二进制数据是唯一的方式。 
- 代码：查询和文档中可以包括任何JavaScript代码，{“x”:function(){/*…*/}} 

## 常用语法

### 创建数据库

选择和创建数据库的语法格式： 

`use 数据库名称 `

如果数据库不存在则自动创建 

以下语句创建spit数据库 

`use spitdb `

### 插入与查询文档

插入

```mongo
db.集合名称.insert(数据);
```

```
db.spit.insert({content:"听说十次方课程很给力呀",userid:"1011",nickname:"小 雅",visits:NumberInt(902)})
```

查询

```
db.集合名称.find()
db.spit.find()
db.spit.find({userid:'1013'})
db.spit.findOne({userid:'1013'})
db.spit.find().limit(3)
```

### 修改与删除文档

修改

```
db.集合名称.update(条件,修改后的数据)
db.spit.update({_id:"1"},{visits:NumberInt(1000)}) # 会清除未插入的字段
db.spit.update({_id:"2"},{$set:{visits:NumberInt(2000)}}) # 只更新需要更新的字段
```

删除

```
db.集合名称.remove(条件)
db.spit.remove({})
db.spit.remove({visits:1000})
```

### 统计条数

```
db.spit.count()
db.spit.count({userid:"1013"})
```

### 模糊查询

```
db.spit.find({content:/流量/}) # / /为JS中的正则表达式写法
db.spit.find({content:/^加班/})
```

### 大于、小于、不等于

```
db.集合名称.find({ "field" : { $gt: value }}) 
// 大于: field > value db.集合名称.find({ "field" : { $lt: value }}) 
// 小于: field < value db.集合名称.find({ "field" : { $gte: value }}) 
// 大于等于: field >= value db.集合名称.find({ "field" : { $lte: value }}) 
// 小于等于: field <= value db.集合名称.find({ "field" : { $ne: value }}) 
// 不等于: field != value
```
示例：查询吐槽浏览量大于1000的记录
```
db.spit.find({visits:{$gt:1000}})
```

### 包含与不包含

包含使用$in操作符。 

示例：查询吐槽集合中userid字段包含1013和1014的文档 

```
db.spit.find({userid:{$in:["1013","1014"]}})
```

不包含使用$nin操作符。 

示例：查询吐槽集合中userid字段不包含1013和1014的文档 

```
db.spit.find({userid:{$nin:["1013","1014"]}})
```

### 条件连接

我们如果需要查询同时满足两个以上条件，需要使用$and操作符将条件进行关联。（相 

当于SQL的and） 

格式为： 

```
$and:[ { },{ },{ } ]
```

示例：查询吐槽集合中visits大于等于1000 并且小于2000的文档 

```
db.spit.find({$and:[ {visits:{$gte:1000}} ,{visits:{$lt:2000} }]})
```

如果两个以上条件之间是或者的关系，我们使用 操作符进行关联，与前面and的使用方式相同 

格式为： 

```
$or:[ { },{ },{ } ]
```

示例：查询吐槽集合中userid为1013，或者浏览量小于2000的文档记录 

```
db.spit.find({$or:[ {userid:"1013"} ,{visits:{$lt:2000} }]})
```

### 列值增长

```
db.spit.update({_id:"2"},{$inc:{visits:NumberInt(1)}} )
```

## Spring Data Mongodb的基本用法

```xml
<dependency> 
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter‐data‐mongodb</artifactId>
</dependency>
```

```yml
server: port: 9006 
spring: 
	application: 
		name: tensquare‐spit #指定服务名 
data: 
	mongodb: 
		host: 192.168.184.134 
		database: spitdb
```

### 增删改查

```java
/**
 * 吐槽数据访问接口
 */
public interface SpitDao extends MongoRepository<Spit,String> {

    /**
     * 根据上级ID查询吐槽列表
     * @param parentid
     * @param pageable
     * @return
     */
    Page<Spit> findByParentid(String parentid, Pageable pageable);
}
```

其余和JPA基本相同

### 高级操作 MongoTemplate

示例1：更新点赞

```java
    /**
     * 点赞
     * @param id
     */
    public void updateThumnup(String id){
        // 条件
        Query query = new Query();
        query.addCriteria(Criteria.where("_id").is(id));
        // 更新的值
        Update update = new Update();
        update.inc("thumbup", 1);
        // 执行更新
        mongoTemplate.updateFirst(query, update, "spit");
    }
```



