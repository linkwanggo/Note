# NPM包资源管理器入门

## 1.1 什么是NPM

npm全称Node Package Manager，他是node包管理和分发工具。其实我们可以把NPM理解为前端的Maven .

我们通过npm 可以很方便地下载js库，管理前端工程.

最近版本的node.js已经集成了npm工具，在命令提示符输入 npm -v 可查看当前npm版本

## 1.2 NPM命令

### 1.2.1 初始化工程

init命令是工程初始化命令。

建立一个空文件夹，在命令提示符进入该文件夹  执行命令初始化

```sh
npm init
```

按照提示输入相关信息，如果是用默认值则直接回车即可。

name: 项目名称

version: 项目版本号

description: 项目描述

keywords: {Array}关键词，便于用户搜索到我们的项目

最后会生成package.json文件，这个是包的配置文件，相当于maven的pom.xml

我们之后也可以根据需要进行修改。

### 1.2.2 本地安装

install命令用于安装某个模块，如果我们想安装express模块（node的web框架），输出命令如下：

```
npm install express
```

出现黄色的是警告信息，可以忽略，请放心，你已经成功执行了该命令。

在该目录下已经出现了一个node_modules文件夹 和package-lock.json

node_modules文件夹用于存放下载的js库（相当于maven的本地仓库）

package-lock.json是当 node_modules 或 package.json 发生变化时自动生成的文件。这个文件主要功能是确定当前安装的包的依赖，以便后续重新安装的时候生成相同的依赖，而忽略项目开发过程中有些依赖已经发生的更新。

我们再打开package.json文件，发现刚才下载的express已经添加到依赖列表中了.

关于版本号定义：

```
指定版本：比如1.2.2，遵循“大版本.次要版本.小版本”的格式规定，安装时只安装指定版本。

波浪号（tilde）+指定版本：比如~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2），但是不安装1.1.x，也就是说安装时不改变大版本号和次要版本号。

插入号（caret）+指定版本：比如ˆ1.2.2，表示安装1.x.x的最新版本（不低于1.2.2），但是不安装2.x.x，也就是说安装时不改变大版本号。需要注意的是，如果大版本号为0，则插入号的行为与波浪号相同，这是因为此时处于开发阶段，即使是次要版本号变动，也可能带来程序的不兼容。

latest：安装最新版本。
```

### 1.2.3 全局安装

刚才我们使用的是本地安装，会将js库安装在当前目录，而使用全局安装会将库安装到你的全局目录下。

如果你不知道你的全局目录在哪里，执行命令

```
npm root -g
```

我的全局目录在

C:\Users\Administrator\AppData\Roaming\npm\node_modules

比如我们全局安装jquery,  输入以下命令

```sh
npm install jquery -g
```

### 1.2.4 批量下载

我们从网上下载某些代码，发现只有package.json,没有node_modules文件夹，这时我们需要通过命令重新下载这些js库.

进入目录（package.json所在的目录）输入命令

```sh
npm install
```

此时，npm会自动下载package.json中依赖的js库.

### 1.2.5淘宝NPM镜像

有时我们使用npm下载资源会很慢，所以我们可以安装一个cnmp(淘宝镜像)来加快下载速度。

输入命令，进行全局安装淘宝镜像。

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

安装后，我们可以使用以下命令来查看cnpm的版本

```
cnpm -v
```

使用cnpm

```
cnpm install 需要下载的js库
```



### 1.2.6 运行工程

如果我们想运行某个工程，则使用run命令

如果package.json中定义的脚本如下



dev是开发阶段测试运行

build是构建编译工程

lint 是运行js代码检测 

我们现在来试一下运行dev

```sh
npm run dev
```

### 1.2.7 编译工程

我们接下来，测试一个代码的编译.编译后我们就可以将工程部署到nginx中啦~

编译后的代码会放在dist文件夹中，首先我们先删除dist文件夹中的文件,进入命令提示符输入命令

```sh
npm run build
```

生成后我们会发现只有个静态页面，和一个static文件夹

这种工程我们称之为单页Web应用（single page web application，SPA），就是只有一张Web页面的应用，是加载单个HTML 页面并在用户与应用程序交互时动态更新该页面的Web应用程序。

这里其实是调用了webpack来实现打包的，关于webpack我们后续的章节进行介绍