# ES6新特性入门

## 1.1 什么是ES6

编程语言JavaScript是ECMAScript的实现和扩展 。ECMAScript是由ECMA（一个类似W3C的标准组织）参与进行标准化的语法规范。ECMAScript定义了：

[语言语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar) – 语法解析规则、关键字、语句、声明、运算符等。

[类型](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures) – 布尔型、数字、字符串、对象等。

[原型和继承](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

内建对象和函数的[标准库](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects) – [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)、[Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)、[数组方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)、[对象自省方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)等。

ECMAScript标准不定义HTML或CSS的相关功能，也不定义类似DOM（文档对象模型）的[Web API](https://developer.mozilla.org/en-US/docs/Web/API)，这些都在独立的标准中进行定义。ECMAScript涵盖了各种环境中JS的使用场景，无论是浏览器环境还是类似[node.js](http://nodejs.org/)的非浏览器环境。

ECMAScript标准的历史版本分别是1、2、3、5。

那么为什么没有第4版？其实，在过去确实曾计划发布提出巨量新特性的第4版，但最终却因想法太过激进而惨遭废除（这一版标准中曾经有一个极其复杂的支持泛型和类型推断的内建静态类型系统）。

ES4饱受争议，当标准委员会最终停止开发ES4时，其成员同意发布一个相对谦和的ES5版本，随后继续制定一些更具实质性的新特性。这一明确的协商协议最终命名为“Harmony”，因此，ES5规范中包含这样两句话

> ECMAScript是一门充满活力的语言，并在不断进化中。
>
> 未来版本的规范中将持续进行重要的技术改进

2009年发布的改进版本ES5，引入了[Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)、[Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)、[getters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get)和[setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set)、[严格模式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)以及[JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)对象。

ECMAScript 1.0（以下简称ES6）是JavaScript语言的下一代标准，2015年6月正式发布。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

## 1.2 Node.js中使用ES6

ES6+ 太棒了,但是很多高级功能node是不支持的,就需要使用babel转换成ES5

 （1）babel转换配置,项目根目录添加.babelrc 文件

```json
{
  "presets" : ['es2015']
}
```

（2）安装es6转换模块

```
cnpm install babel-preset-es2015 --save-dev
```

（3）全局安装命令行工具

```
cnpm install  babel-cli -g
```

（4）使用

```
babel-node js文件名
```



## 1.3 语法新特性

### 1.3.1 变量声明let

我们都是知道在ES6以前，var关键字声明变量。无论声明在何处，都会被视为声明在函数的最顶部(不在函数内即在全局作用域的最顶部)。这就是函数变量提升例如

```js
  function aa() {
    if(bool) {
        var test = 'hello man'
    } else {
        console.log(test)
    }
  }
```

以上的代码实际上是:

```js
function aa() {
    var test // 变量提升
    if(bool) {
        test = 'hello man'
    } else {
        //此处访问test 值为undefined
        console.log(test)
    }
    //此处访问test 值为undefined
  }
```

所以不用关心bool是否为true or false。实际上，无论如何test都会被创建声明。

接下来ES6主角登场：

我们通常用let和const来声明，let表示变量、const表示常量。let和const都是块级作用域。怎么理解这个块级作用域？在一个函数内部 ，在一个代码块内部。看以下代码

```js
 function aa() {
    if(bool) {
       let test = 'hello man'
    } else {
        //test 在此处访问不到
        console.log(test)
    }
  }
```

### 1.3.2 常量声明

const 用于声明常量，看以下代码

```js
const name = 'lux'
name = 'joe' //再次赋值此时会报错
```



### 1.3.3 模板字符串

es6模板字符简直是开发者的福音啊，解决了ES5在字符串功能上的痛点。

第一个用途，基本的字符串格式化。将表达式嵌入字符串中进行拼接。用${}来界定。

```js
    //es5 
    var name = 'lux'
    console.log('hello' + name)
    //es6
    const name = 'lux'
    console.log(`hello ${name}`) //hello lux
```

第二个用途，在ES5时我们通过反斜杠(\)来做多行字符串或者字符串一行行拼接。ES6反引号(``)直接搞定。

```js
    // es5
    var msg = "Hi \
    man!"
    // es6
    const template = `<div>
        <span>hello world</span>
    </div>`
```

### 1.3.4 函数默认参数

ES6为参数提供了默认值。在定义函数时便初始化了这个参数，以便在参数没有被传递进去时使用。

看例子代码

```js
    function action(num = 200) {
        console.log(num)
    }
    action() //200
    action(300) //300
```

### 1.3.5 箭头函数

ES6很有意思的一部分就是函数的快捷写法。也就是箭头函数。

箭头函数最直观的三个特点。

1不需要function关键字来创建函数

2省略return关键字

3继承当前上下文的 this 关键字

看下面代码（ES6）

```js
 (response,message) => {
    .......
 }
```

相当于ES5代码

```js
function(response,message){
    ......
}
```

### 1.3.6 对象初始化简写

ES5我们对于对象都是以键值对的形式书写，是有可能出现键值对重名的。例如

```js
function people(name, age) {
        return {
            name: name,
            age: age
        };
    }
```

以上代码可以简写为

```
  function people(name, age) {
        return {
            name,
            age
        };
    }
```

### 1.3.7 解构

数组和对象是JS中最常用也是最重要表示形式。为了简化提取信息，ES6新增了解构，这是将一个数据结构分解为更小的部分的过程

ES5我们提取对象中的信息形式如下

```js
    const people = {
        name: 'lux',
        age: 20
    }
    const name = people.name
    const age = people.age
    console.log(name + ' --- ' + age)
```

是不是觉得很熟悉，没错，在ES6之前我们就是这样获取对象信息的，一个一个获取。现在，ES6的解构能让我们从对象或者数组里取出数据存为变量，例如

```js
//对象
    const people = {
        name: 'lux',
        age: 20
    }
    const { name, age } = people
    console.log(`${name} --- ${age}`)
    //数组
    const color = ['red', 'blue']
    const [first, second] = color
    console.log(first) //'red'
    console.log(second) //'blue'
```

### 1.3.8 Spread Operator

ES6中另外一个好玩的特性就是Spread Operator 也是三个点儿...接下来就展示一下它的用途。 组装对象或者数组

```js
    //数组
    const color = ['red', 'yellow']
    const colorful = [...color, 'green', 'pink']
    console.log(colorful) //[red, yellow, green, pink]
    
    //对象
    const alp = { fist: 'a', second: 'b'}
    const alphabets = { ...alp, third: 'c' }
    console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"
```

### 1.3.9 import 和 export

import导入模块、export导出模块

lib.js

```js
let fn0=function(){
    console.log('fn0...');
}
export {fn0}
```

demo9.js

```js
import {fn0} from './lib'
fn0();
```

注意：node(v8.x)本身并不支持import关键字，所以我们需要使用babel的命令行工具来执行（配置详见1.2小节内容）

```
babel-node demo9
```

