# Docker下部署nginx

## 1. 下载镜像

[镜像下载地址](https://hub.docker.com/)

选择自己机型对应的镜像

```
docker pull arm64v8/nginx:1.17-alpine
```

## 2. 创建映射文件

将本地的配置文件映射到docker容器中的nginx必然首先在本地创建配置文件

### 2.1 创建nginx测试实例

```
docker run --rm --name nginx-test -p 8080:80 -d nginx
```

--rm表示镜像停止后自动删除实例

### 2.2 拷贝配置镜像中的配置文件到本地

```
docker exec -it nginx-test /bin/sh # 进入容器的命令可能稍有不同
```

```
mkdir -p /usr/share/nginx /etc/nginx/nginx.conf /var/log/nginx
```

```
docker cp nginx-test:/etc/nginx/nginx.conf /etc/nginx/
docker cp nginx-test:/usr/share/nginx /usr/share
docker cp nginx-test:/var/log/nginx /var/log
```

## 3. 运行镜像


```
docker run -d -p 801:80 --name nginx -v /etc/nginx/nginx.conf:/etc/nginx/nginx.conf -v /usr/share/nginx:/usr/share/nginx -v /var/log/nginx:/var/log/nginx arm64v8/nginx:1.17-alpine
```

## 4. 修改本地html查看是否映射成功

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<h2>This message from Debian Cool!</h2> <!-- 这是我添加的内容 -->
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

成功！

![nginx](https://tva1.sinaimg.cn/large/00831rSTly1gch2hd0ee1j30ii084jsf.jpg)

# ---END---