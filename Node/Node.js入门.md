# Node.js入门

## 1.1 什么是Node.js

简单的说 Node.js 就是运行在服务端的 JavaScript。

Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。

Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

## 1.2 Node.js安装

1、下载对应你系统的Node.js版本:

[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

（我们现在使用的版本是8.9.4，资源中也已提供）
2、选安装目录进行安装

默认即可

3.测试

在命令提示符下输入命令

```
node -v
```

会显示当前node的版本

## 1.3 快速入门

### 1.3.1 控制台输出

我们现在做个最简单的小例子，演示如何在控制台输出，创建文本文件demo1.js,代码内容

```js
var a=1;
var b=2;
console.log(a+b);
```

我们在命令提示符下输入命令

```shell
node demo1.js
```

### 1.3.2 使用函数

创建文本文件demo1.js

```js
var c=add(100,200);
console.log(c);
function add(a,b){
	return a+b;
}
```

命令提示符输入命令

```sh
node demo1.js
```

运行后看到输出结果为300

### 1.3.3 模块化编程

创建文本文件demo3_1.js

```js
exports.add=function(a,b){
	return a+b;
}
```

创建文本文件demo3_1.js

```js
var demo= require('./demo3_1');
console.log(demo.add(400,600));
```

我们在命令提示符下输入命令

```sh
node demo3_1.js
```

结果为1000

### 1.3.4 创建web服务器

创建文本文件demo4.js

```js
var http = require('http');
http.createServer(function (request, response) {
    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});
    // 发送响应数据 "Hello World"
    response.end('Hello World\n');
}).listen(8888);
// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```

http为node内置的web模块

我们在命令提示符下输入命令

```sh
node demo4.js
```

服务启动后，我们打开浏览器，输入网址

[http://localhost:8888/](http://localhost:8888/)

即可看到网页输出结果Hello World

心情是不是很激动呢？Ctrl+c 终止运行。

### 1.3.5 理解服务端渲染

我们创建demo5.js  ，将上边的例子写成循环的形式

```js
var http = require('http');
http.createServer(function (request, response) {
    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});
    // 发送响应数据 "Hello World"
	for(var i=0;i<10;i++){
		response.write('Hello World\n');
	}  
	response.end('');	
}).listen(8888);
// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```

我们在命令提示符下输入命令启动服务

```sh
node demo5.js
```

浏览器地址栏输入http://127.0.0.1:8888即可看到查询结果。

我们右键“查看源代码”发现，并没有我们写的for循环语句，而是直接的10条Hello World ，这就说明这个循环是在服务端完成的，而非浏览器（客户端）来完成。这与我们原来的JSP很是相似。

### 1.3.6 接收参数

创建demo6.js

```js
var http = require('http');
var url = require('url');
http.createServer(function(request, response){
    response.writeHead(200, {'Content-Type': 'text/plain'});
    // 解析 url 参数
    var params = url.parse(request.url, true).query;
    response.write("name:" + params.name);
    response.write("\n");
    response.end();
}).listen(8888);
console.log('Server running at http://127.0.0.1:8888/');
```

我们在命令提示符下输入命令

```sh
node demo6.js
```

在浏览器测试结果