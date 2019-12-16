# CentOS7安装Nginx（1.16）的方式

## 1.  添加Nginx存储库

```shell
sudo yum -y install epel-release
```

## 2. 安装Nginx

现在Nginx存储库已经安装在您的服务器上，使用以下`yum`命令安装Nginx ：

```shell
sudo yum -y install nginx
```

## 3. 启动Nginx

```shell
sudo systemctl start nginx
sudo systemctl enable nginx # 系统运行时启动nginx
```

## 4. 如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：(如果防火墙关了,可直接跳过)

### 4.1 允许http通信

```shell
sudo firewall-cmd --permanent --zone=public --add-service=http
```

### 4.2 允许https通信

```shell
sudo firewall-cmd --permanent --zone=public --add-service=https
```

### 4.3 重启防火墙

```shell
sudo firewall-cmd --reload
```

## 5. 测试

输入192.168.2.20查看

![](https://i.loli.net/2019/11/07/QhPaTZOySUGV2iX.png)

## 6. 卸载

### 6.1 停止Nginx软件

```shell
# 具体方式请自行选择
service nginx stop
systemctl stop nginx
systemctl stop nginx.service
```

### 6.2 关闭开机启动

```shell
systemctl disable nginx
systemctl disable nginx.service
```

### 6.3 从源头删除Nginx

```shell
whereis nginx
rm 删除全部
yum remove nginx
```

# ---END---

