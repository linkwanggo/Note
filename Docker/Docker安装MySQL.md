# Docker安装MySQL

## 1. 下载镜像

```
docker pull arm64v8/mariadb:10.1
```

## 2. 启动镜像

```bash
docker run --name mysql -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /etc/mysql/conf.d:/etc/mysql/conf.d arm64v8/mariadb:10.1
```

## 3. 允许外网(局域网)访问

```bash
docker exec -it mysql /bin/bash
```

```
1.登录数据库
mysql -u root -p123456

输入密码后，执行命令：
mysql> use mysql;

2.查询host
mysql> select user,host from user;

3.创建host
如果没有"%"这个host值,就执行下面这两句:
mysql> update user set host=’%’ where user=‘root’;
mysql> flush privileges;

4.授权用户
（1）任意主机以用户root和密码mypwd连接到mysql服务器
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
mysql> flush privileges;

（2）IP为192.168.1.XXX的主机以用户myuser和密码mypwd连接到mysql服务器
mysql> GRANT ALL PRIVILEGES ON . TO ‘myuser’@‘192.168.1.XXX’ IDENTIFIED BY ‘mypwd’ WITH GRANT OPTION;
mysql> flush privileges;
```

## ---END---