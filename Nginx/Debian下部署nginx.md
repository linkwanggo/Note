# Debian下部署nginx

## 问题

理论上debian的apt源中存在nginx,因此我们只需要

```bash
apt install nginx
```

**但是就是这么不幸，使用这个命令在我的Debian上会报错**

怎么办？

## 解决方案

更新nginx源

```bash
apt install gnupg2
wget http://nginx.org/keys/nginx_signing.key && sudo apt-key add nginx_signing.key
sudo apt update && apt install nginx -y
```

**到这一步就安装完成 没有问题不用看之后的内容**



**如果出现安装的 nginx 和系统自带的 nginx 的配置目录有区别, 进行如下方式修改（本人未尝试）**

```
sudo mkdir /etc/nginx/{sites-available,sites-enabled}
sudo mv /etc/nginx/conf.d/* /etc/nginx/sites-available
sudo rmdir -f /etc/nginx/conf.d/
sudo perl -pi -e 's/conf.d/sites-enabled/g' /etc/nginx/nginx.conf
```

## 成功截图

![image-20200411221049273](https://tva1.sinaimg.cn/large/007S8ZIlly1gdq6mkhd4oj30p80f6ab0.jpg)

## ---END---