# python gunicorn nginx部署flask项目

## 1. 安装virtualenv

[如何安装virtualenv]()

## 2. 创建虚拟环境

```bash
mkvirtualenv mapleleaf  # 项目名
workon mapleleaf
```

## 3. 安装依赖模块

项目使用pipfile来管理包

进入项目所在路径

```
pip3 install pipenv
pipenv install Pipfile
```

## 安装gunicorn

```
pip3 install gunicorn
```

## 运行项目

```
gunicorn mapleleaf:app --workers 4 --bind 127.0.0.1:5000
```

## 使用nginx代理项目

### [安装nginx]()

在conf.d中新增`mapleleaf.conf`

```
server {
    listen 80;

    server_name linkwanggo.tk www.linkwanggo.tk;

    access_log  /var/log/nginx/mapleleaf_access.log;
    error_log  /var/log/nginx/mapleleaf_error.log;

    location / {
        proxy_pass         http://127.0.0.1:5000/;
        proxy_redirect     off;

        proxy_set_header   Host                 $host;
        proxy_set_header   X-Real-IP            $remote_addr;
        proxy_set_header   X-Forwarded-For      $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto    $scheme;
    }
}
```

**到此基本就完成项目部署**

## 成功截图

![image-20200411222827146](https://tva1.sinaimg.cn/large/007S8ZIlly1gdq74u54l2j30h60ac74p.jpg)

## ---END---