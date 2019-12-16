# FileZilla连接CentOS7出现服务器拒绝的解决办法

## 1. 点击文件--->站点管理器

将协议改为SFTP

## 2. 开启21端口

```bash
sudo firewall-cmd --permanent --add-port=21/tcp
sudo firewall-cmd --reload
```



# ---END---