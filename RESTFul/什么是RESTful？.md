# 什么是RESTful？

## 1.1 什么是RESTful架构

​	RESTful架构，就是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用。REST这个词，是[Roy Thomas Fielding](http://en.wikipedia.org/wiki/Roy_Fielding)在他2000年的[博士论文](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)中提出的

![](https://tva1.sinaimg.cn/large/006tNbRwly1gaxa9416xuj308m0abjxy.jpg)

​	Fielding是一个非常重要的人，他是HTTP协议（1.0版和1.1版）的主要设计者、Apache服务器软件的作者之一、Apache基金会的第一任主席。所以，他的这篇论文一经发表，就引起了关注，并且立即对互联网开发产生了深远的影响。

​	Fielding将他对互联网软件的架构原则，定名为REST，即Representational State Transfer的缩写。我对这个词组的翻译是"表现层状态转化"。如果一个架构符合REST原则，就称它为RESTful架构。

## 1.2 理解RESTful架构 	

**要理解RESTful架构，最好的方法就是去理解Representational State Transfer这个词组到底是什么意思，它的每一个词代表了什么涵义。**

（1）**资源（Resources）**

REST的名称"表现层状态转化"中，省略了主语。"表现层"其实指的是"资源"（Resources）的"表现层"。

**所谓"资源"，就是网络上的一个实体，或者说是网络上的一个具体信息。**它可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的实在。你可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的URI。要获取这个资源，访问它的URI就可以，因此URI就成了每一个资源的地址或独一无二的识别符。

所谓"上网"，就是与互联网上一系列的"资源"互动，调用它的URI。

（2）**表现层（Representation）**

"资源"是一种信息实体，它可以有多种外在表现形式。**我们把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）。**

比如，文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现。

URI只代表资源的实体，不代表它的形式。严格地说，有些网址最后的".html"后缀名是不必要的，因为这个后缀名表示格式，属于"表现层"范畴，而URI应该只代表"资源"的位置。它的具体表现形式，应该在HTTP请求的头信息中用Accept和Content-Type字段指定，这两个字段才是对"表现层"的描述。

（3）**状态转化（State Transfer）**

访问一个网站，就代表了客户端和服务器的一个互动过程。在这个过程中，势必涉及到数据和状态的变化。

互联网通信协议HTTP协议，是一个无状态协议。这意味着，所有的状态都保存在服务器端。因此，**如果客户端想要操作服务器，必须通过某种手段，让服务器端发生"状态转化"（State Transfer）。而这种转化是建立在表现层之上的，所以就是"表现层状态转化"。**

客户端用到的手段，只能是HTTP协议。具体来说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：**GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。**



综合上面的解释，我们总结一下什么是RESTful架构：

　　（1）每一个URI代表一种资源；

　　（2）客户端和服务器之间，传递这种资源的某种表现层 (GET形式表现、POST形式表现......)；

​    　（3）客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。

## 1.3 RESTful的特征

面向资源是REST最明显的特征，对于同一个资源的一组不同的操作。资源是服务器 

上一个可命名的抽象概念，资源是以名词为核心来组织的，首先关注的是名词。REST要 

求，必须通过统一的接口来对资源执行各种操作。对于每个资源只能执行一组有限的操 

作。

7个HTTP方法：GET/POST/PUT/DELETE/PATCH/HEAD/OPTIONS

## 1.4 常见错误

（1）URI包含动词

```
POST /accounts/1/transfer/500/to/2
```

正确的写法是把动词transfer改成名词transaction

（2）URI包含版本

```
http://www.example.com/app/1.0/foo

http://www.example.com/app/1.1/foo

http://www.example.com/app/2.0/foo
```

因为不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一个URI。版本号可以在HTTP请求头信息的Accept字段中进行区分

```
Accept: vnd.example-com.foo+json; version=1.0

Accept: vnd.example-com.foo+json; version=1.1

Accept: vnd.example-com.foo+json; version=2.0
```

## 1.5 RESTful接口规范

### 1.5.1 GET

* **安全且幂等** 

* 获取表示 

* 变更时获取表示（缓存） 

* 200（OK） - 表示已在响应中发出 

- 204（无内容） - 资源有空表示 

- 301（Moved Permanently） - 资源的URI已被更新 

- 303（See Other） - 其他（如，负载均衡） 

- 304（not modified）- 资源未更改（缓存） 

- 400 （bad request）- 指代坏请求（如，参数错误） 

- 404 （not found）- 资源不存在 

- 406 （not acceptable）- 服务端不支持所需表示 

- 500 （internal server error）- 通用错误响应 

- 503 （Service Unavailable）- 服务端当前无法处理请求 

  Version:0.9 StartHTML:0000000105 EndHTML:0000010185 StartFragment:0000000141 EndFragment:0000010145   

### 1.5.2 POST

- **不安全且不幂等** 

-   使用服务端管理的（自动产生）的实例号创建资源 

-   创建子资源 

-   北京市昌平区建材城西路金燕龙办公楼一层 电话：400-618-9090部分更新资源 

-   如果没有被修改，则不过更新资源（乐观锁） 

-   200（OK）- 如果现有资源已被更改 

-   201（created）- 如果新资源被创建 

-   202（accepted）- 已接受处理请求但尚未完成（异步处理） 

-   301（Moved Permanently）- 资源的URI被更新 

-   303（See Other）- 其他（如，负载均衡） 

-   400（bad request）- 指代坏请求 

-   404 （not found）- 资源不存在 

-   406 （not acceptable）- 服务端不支持所需表示 

-   409 （conflict）- 通用冲突 

-   412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突） 

-   415 （unsupported media type）- 接受到的表示不受支持 

-   500 （internal server error）- 通用错误响应 

-   503 （Service Unavailable）- 服务当前无法处理请求 

Version:0.9 StartHTML:0000000105 EndHTML:0000015262 StartFragment:0000000141 EndFragment:0000015222   

### 1.5.3 PUT

- **不安全但幂等** 
- 用客户端管理的实例号创建一个资源 

- 通过替换的方式更新资源 

- 如果未被修改，则更新资源（乐观锁） 

- 200 （OK）- 如果已存在资源被更改 

- 201 （created）- 如果新资源被创建 

- 301（Moved Permanently）- 资源的URI已更改 

- 303 （See Other）- 其他（如，负载均衡） 

- 400 （bad request）- 指代坏请求 

- 404 （not found）- 资源不存在 

- 406 （not acceptable）- 服务端不支持所需表示 

- 409 （conflict）- 通用冲突 

- 412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突） 

- 415 （unsupported media type）- 接受到的表示不受支持 
- 500 （internal server error）- 通用错误响应 
- 503 （Service Unavailable）- 服务当前无法处理请求 

### 1.5.4 DELETE 

- **不安全但幂等** 
- 删除资源 
- 200 （OK）- 资源已被删除 

- 301 （Moved Permanently）- 资源的URI已更改 
- 303 （See Other）- 其他，如负载均衡 

- 400 （bad request）- 指代坏请求 

- 404 （not found）- 资源不存在 

- 409 （conflict）- 通用冲突 

- 500 （internal server error）- 通用错误响应 

- 503 （Service Unavailable）- 服务端当前无法处理请求 