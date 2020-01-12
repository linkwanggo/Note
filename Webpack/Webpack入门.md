# Webpack入门

## 1.1 什么是Webpack

​	Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

![](https://tva1.sinaimg.cn/large/006tNbRwgy1gatyav1qtnj30fe07lwfh.jpg) 

​	从图中我们可以看出，Webpack 可以将多种静态资源 js、css、less 转换成一个静态文件，减少了页面的请求。  接下来我们简单为大家介绍 Webpack 的安装与使用

## 1.2 Webpack安装

全局安装

```
npm install webpack -g
npm install webpack-cli -g
```

安装后查看版本号

```
webpack -v
```

## 1.3 快速入门

### 1.3.1  JS打包

（1）创建src文件夹，创建bar.js

```js
exports.info=function(str){
   document.write(str);
}
```

（2）src下创建logic.js

```js
exports.add=function(a,b){
	return a+b;
}
```

（3）src下创建main.js

```js
var bar= require('./bar');
var logic= require('./logic');
bar.info( 'Hello world!'+ logic.add(100,200));
```

（4）创建配置文件webpack.config.js  ，该文件与src处于同级目录

```js
var path = require("path");
module.exports = {
	entry: './src/main.js',
	output: {
		path: path.resolve(__dirname, './dist'),
		filename: 'bundle.js'
	}
};
```

以上代码的意思是：读取当前目录下src文件夹中的main.js（入口文件）内容，把对应的js文件打包，打包后的文件放入当前目录的dist文件夹下，打包后的js文件名为bundle.js

（5）执行编译命令

```
webpack
```

执行后查看bundle.js 会发现里面包含了上面两个js文件的内容

（7）创建index.html ,引用bundle.js

```html
<!doctype html>
<html>
  <head>  
  </head>
  <body>   
    <script src="dist/bundle.js"></script>
  </body>
</html>
```

测试调用index.html，会发现有内容输出

### 1.3.2 CSS打包

（1）安装style-loader和 css-loader

Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。

Loader 可以理解为是模块和资源的转换器，它本身是一个函数，接受源文件作为参数，返回转换的结果。这样，我们就可以通过 require 来加载任何类型的模块或文件，比如 CoffeeScript、 JSX、 LESS 或图片。首先我们需要安装相关Loader插件，css-loader 是将 css 装载到 javascript；style-loader 是让 javascript 认识css

```
cnpm install style-loader css-loader --save-dev
```

（2）修改webpack.config.js

```js
var path = require("path");
module.exports = {
	entry: './src/main.js',
	output: {
		path: path.resolve(__dirname, './dist'),
		filename: 'bundle.js'
	},
	module: {
		rules: [  
            {  
                test: /\.css$/,  
                use: ['style-loader', 'css-loader']
            }  
        ]  
	}
};
```

（3）在src文件夹创建css文件夹,css文件夹下创建css1

```css
body{
 background:red;
}
```

（4）修改main.js ，引入css1.css

```
require('./css1.css');
```

（5）重新运行webpack

（6）运行index.html看看背景是不是变成红色啦？