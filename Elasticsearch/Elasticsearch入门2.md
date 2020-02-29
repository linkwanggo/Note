# Elasticsearch入门

## 简介

- 了解Elasticsearch的特点及体系结构 
- 完成Elasticsearch安装，能够调用RestAPI完成基本增删改查操作 
- 完成IK分词器的安装，能够使用IK分词器进行分词 
- 使用SpringDataElasticsearch完成搜索微服务的开发（重点） 
- 使用logstash完成mysql与Elasticsearch的同步工作 

### 什么是elasticsearch

Elasticsearch是一个实时的分布式搜索和分析引擎。它可以帮助你用前所未有的速 

度去处理大规模数据。ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分 

布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发 

的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用 

于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。 

### elasticsearch的特点

（1）可以作为一个大型分布式集群（数百台服务器）技术，处理PB级数据，服务大公 

司；也可以运行在单机上 

（2）将全文检索、数据分析以及分布式技术，合并在了一起，才形成了独一无二的ES； 

（3）开箱即用的，部署简单 

（4）全文检索，同义词处理，相关度排名，复杂数据分析，海量数据的近实时处理

### elasticsearch的体系结构

下表是Elasticsearch与MySQL数据库逻辑结构概念的对比 

| Elasticsearch  | 关系型数据库**Mysql** |
| -------------- | --------------------- |
| 索引(index)    | 数据库(databases)     |
| 类型(type)     | 表(table)             |
| 文档(document) | 行(row)               |

## Postman调用RESTAPI

### 新建索引

```
http://127.0.0.1:9200/articleindex/
```

### 新建文档

以post方式提交 http://127.0.0.1:9200/articleindex/article

body

 ```
{ "title":"SpringBoot2.0", "content":"发布啦" }
 ```

### 查询全部文档

查询某索引某类型的全部数据，以get方式请求 

http://127.0.0.1:9200/articleindex/article/_search

### 修改文档

以put形式提交以下地址： 

http://192.168.184.134:9200/articleindex/article/AWPKrI4pFdLZnId5S_F7 

```
{ "title":"SpringBoot2.0正式版", "content":"发布了吗" }
```

如果我们在地址中的ID不存在，则会创建新文档 

以put形式提交以下地址： 

http://192.168.184.134:9200/articleindex/article/1 

```
{ "title":"十次方课程好给力", "content":"知识点很多" }
```

### 按ID查询文档

GET方式请求

http://192.168.184.134:9200/articleindex/article/1  

### 基本匹配查询

根据某列进行查询 get方式提交下列地址： 

http://192.168.184.134:9200/articleindex/article/_search?q=title:十次方课程 

好给力 

### 模糊查询

我们可以用*代表任意字符： 

```
http://192.168.184.134:9200/articleindex/article/_search?q=title:*s*
```

### 删除文档

根据ID删除文档,删除ID为1的文档 DELETE方式提交 

```
http://192.168.184.134:9200/articleindex/article/1
```

## IK分词器

默认的中文分词是将每个字看成一个词，这显然是不符合要求的，所以我们需要安装中 

文分词器来解决这个问题。 

IK分词是一款国人开发的相对简单的中文分词器。虽然开发者自2012年之后就不在维护 

了，但在工程应用中IK算是比较流行的一款！我们今天就介绍一下IK中文分词器的使用。

### IK分词器的安装

下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases 下载5.6.8版 

本 课程配套资源也提供了: 资源\配套软件\elasticsearch\elasticsearch-analysis-ik- 

5.6.8.zip 

（1）先将其解压，将解压后的elasticsearch文件夹重命名文件夹为ik 

（2）将ik文件夹拷贝到elasticsearch/plugins 目录下。 

（3）重新启动，即可加载IK分词器 

### IK分词器测试

IK提供了两个分词算法ik_smart 和 ik_max_word 

其中 ik_smart 为最少切分，ik_max_word为最细粒度划分 

### 自定义词库

默认的分词并没有识别“传智播客”是一个词。如果我们想让系统识别“传智播客”是一个 

词，需要编辑自定义词库。 

步骤： 

（1）进入elasticsearch/plugins/ik/config目录 

（2）新建一个my.dic文件，编辑内容： 

```
传智播客
```

修改IKAnalyzer.cfg.xml（在ik/config目录下）

```xml
<properties> 
  <comment>IK Analyzer 扩展配置</comment> 
  <!‐‐用户可以在这里配置自己的扩展字典 ‐‐> 
  <entry key="ext_dict">my.dic</entry> 
  <!‐‐用户可以在这里配置自己的扩展停止词字典‐‐> 
  <entry key="ext_stopwords"></entry> 
</properties>
```

重新启动elasticsearch即可

