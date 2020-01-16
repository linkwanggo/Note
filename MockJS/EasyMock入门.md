# EasyMock入门

## 1.1 什么是EasyMock

​	Easy Mock 是杭州大搜车无线团队出品的一个极其简单、高效、可视化、并且能快速生成模拟数据的`在线 mock 服务`。以项目管理的方式组织 Mock List，能帮助我们更好的管理 Mock 数据。

地址：https://www.easy-mock.com

在线文档：https://www.easy-mock.com/docs

## 1.2 EasyMock基本入门 

### 1.2.1初始设置

（1）登录或注册。

浏览器打开https://www.easy-mock.com  输出用户名和密码，如果不存在会自动注册。**注意：请牢记密码，系统没有找回密码功能！**

![](/Users/linkwanggo/Movies/28-微服务社交平台十次方/01-前端/前端开发资源/讲义（MD）/image/2-2.png)

登录后进入主界面

![](https://tva1.sinaimg.cn/large/006tNbRwly1gaweklnrl8j30tp0dz3z3.jpg)

（2）创建项目：点击右下角的加号

![](https://tva1.sinaimg.cn/large/006tNbRwly1gawel2wpyaj308m0abjxy.jpg)

填写项目名称，点击创建按钮

![](/Users/linkwanggo/Movies/28-微服务社交平台十次方/01-前端/前端开发资源/讲义（MD）/image/2-5.png)

创建完成后可以在列表中看到刚刚创建的项目

### 1.2.2接口操作

（1）创建接口。点击列表中的项目

![](https://tva1.sinaimg.cn/large/006tNbRwly1gawel6z0buj30ui0bnwev.jpg)

进入项目工作台页面

![](https://tva1.sinaimg.cn/large/006tNbRwly1gawelaij7vj30t00djaan.jpg)

点击“创建接口” ,左侧区域输出mock数据，右侧定义Method 、 Url 、描述等信息。

![](https://tva1.sinaimg.cn/large/006tNbRwly1gaweld7873j310h0f474g.jpg)

我们可以将我们在Mock.js入门案例中的对象放入左侧的编辑窗口

```
{
  'list|10': [{
    "id|+1": 1,
    "name": "@cname",
    "cfirst": "@cfirst",
    "Last": "@Last",
    "point": "@integer",
    "birthday": "@date",
    "pic": "@image",
    "content": "@cword(30,200)",
    "url": "@url",
    "ip": "@ip",
    "email": "@email",
    "region": "@region",
    "county": "@county"
  }]
}
```

填写url   Method  和描述 ，点击创建按钮

（2）克隆接口和修改接口



（3）预览接口和复制接口地址



（4）删除接口



## 1.3 本地部署EasyMock 

### 1.3.1 Centos部署node.js

（1）将node官网下载的node-v8.11.1-linux-x61.tar.xz 上传至服务器

（2）解压xz文件

```
xz -d node-v8.11.1-linux-x61.tar.xz
```

（3）解压tar文件

```
tar -xvf node-v8.11.1-linux-x61.tar
```

（4）目录重命名

```
mv node-v8.11.1-linux-x64 node
```

（5）移动目录到/usr/local下

```
mv node /usr/local/
```

（6）配置环境变量

```
vi /etc/profile
```

填写以下内容

```
#set for nodejs  
export NODE_HOME=/usr/local/node  
export PATH=$NODE_HOME/bin:$PATH
```

执行命令让环境变量生效

```
source /etc/profile
```

查看node版本看是否安装成功

```
node -v
```

### 1.3.2 MongoDB安装与启动

我们使用yum方式安装mongoDb

（1）配置yum

```
vi /etc/yum.repos.d/mongodb-org-3.2.repo
```

编辑以下内容：

```
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

（2）安装MongoDB

```
yum install -y mongodb-org
```

（3）启动MongoD 

```
systemctl start mongod
```



### 1.3.3 Redis安装与启动

（1）下载fedora的epel仓库

```
yum install epel-release
```

（2）下载安装redis

```
yum install redis
```

（3）启动redis服务

```
systemctl start redis
```



### 1.3.4 本地部署easy-mock

（1）项目下载地址：  https://github.com/easy-mock/easy-mock

（2）将easy-mock-dev.zip上传至服务器

（3）安装zip 和unzip 

```
yum install zip unzip
```

（4）解压

```
unzip easy-mock-dev.zip
```

（3）进入其目录，安装依赖

```
npm install
```

（4）执行构建

```
npm run build
```

（5）启动

```
npm run start
```

（6）打开浏览器  http://192.168.181.131:7300

![](https://tva1.sinaimg.cn/large/006tNbRwly1gaweljcgdjj30h10987ee.jpg)

## 1.4 导入SwaggerAPI文档

（1）将我们的SwaggerAPI文档扩展名改为yml

（2）在easyMock中点击“设置”选项卡

（3）SwaggerDocs API 选择Upload 

![](https://tva1.sinaimg.cn/large/006tNbRwly1gawelmk4jxj30dy07gmx5.jpg)

（4）将SwaggerAPI文档拖动到上图的虚线区域，点击保存

（5）回到主界面后点击“同步Swagger”

![](https://tva1.sinaimg.cn/large/006tNbRwly1gawemcggxfj311r0mw429.jpg)