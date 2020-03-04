# 为DDNS绑定域名及80端口屏蔽完美解决方案

## 1. 使用3322 DDNS

[3322网址](http://www.pubyun.com/)

由于3322的DDNS无法使用https，因此本教程基于http进行操作

在3322上绑定公网IP及自动更新（此步骤略过）路由器或者自己写代码都可以！

## 2. 使用github进行端口转发

在github上创建项目: `linkwanggo.github.io` (`xxx.github.io`)

将项目下载到本地

```
git clone git clone https://github.com/linkwanggo/linkwanggo.github.io.git
```

创建并修改index.html

```html
<!DOCTYPE html>
<html>

<head>
  <meta http-equiv="Content-Language" content="zh-CN">
  <meta http-equiv="Content-Type" content="text/html; charset=gb2312">
  <title></title>
</head>
<frameset framespacing="0" border="0" rows="0" frameborder="0">
  <frame name="main" src="http://xxxx.f3322.net:9999/" scrolling="auto" noresize>
</frameset>

</html>
```

上传修改后的代码

```
git add -A
git commit -m "update"
git push
```



同时在github Settings中GitHub Pages设置关闭HTTPS

`Custom domain`添加自定义域名

不勾选`Enforce HTTPS`

![关闭HTTPS](https://tva1.sinaimg.cn/large/00831rSTly1gci0uelqp1j31020hjtaz.jpg)

在域名服务商中将域名指定到`linkwanggo.github.io`

![](https://tva1.sinaimg.cn/large/00831rSTly1gci0wo3hqmj30u20400t8.jpg)

## ---END---

