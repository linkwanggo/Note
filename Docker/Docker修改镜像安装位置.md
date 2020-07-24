# Docker修改镜像安装位置

[参考链接：我使用了最后提供的方案](https://blog.csdn.net/doctorone/article/details/88399533)

江湖有多大，坑就有多多……
我使用的服务器, 系统盘根目录只有20G, 默认Docker 的镜像文件是安装在/var/lib/docker 目录下的, 这样的话我根本装不了太多的镜像，之前遇到一种情况就是docker服务对磁盘的读写操作太大，而且是软连接方式，导致服务器的磁盘不可用，当然测试环境用的是虚拟服务器。 所以这个中情况需要调整一下。

服务器环境：centos7，docker1.12.6

方案1：使用软链接方式（不建议，可以了解一下）
默认情况下Docker的存放位置为：/var/lib/docker

可以通过下面命令查看具体位置：

sudo docker info | grep "Docker RootDir"

解决这个问题，最直接的方法当然是挂载分区到这个目录，但是我的数据盘还有其他东西，这肯定不好管理，所以采用修改镜像和容器的存放路径的方式达到目的。

这个方法里将通过软连接来实现。

 

1.首先停掉Docker服务：
systemctl restart docker

或者

service docker stop

2.对之前的数据做个文件备份
tar -zcC /var/lib/docker >/mnt/var_lib_docker-backup-$(date + %s).tar.gz

3.然后迁移整个/var/lib/docker目录到目的路径：
mv /var/lib/docker /data/tools/docker

4.建立symlink软链接（不会自己Google）
ln -s /data/tools/docke /var/lib/docker

5.确认文件夹类型为symlink 类型
ls -al /var/lib/docker

6.这时候启动Docker时发现存储目录依旧是/var/lib/docker，但是实际上是存储在数据盘的，你可以在数据盘上看到容量变化。
sudo systemctl start docker

方案2：修改镜像和容器的默认存放路径
 

指定镜像和容器存放路径的参数是--graph=/var/lib/docker，我们只需要修改配置文件指定启动参数即可。刚好有个300g盘的挂在/data目录上，所以在这个目录下新建一个文件路径/data/tools/docker

1.Docker的配置文件可以设置大部分的后台进程参数，在各个操作系统中的存放位置不一致，

在 Ubuntu 中的位置是：/etc/default/docker，在 CentOS中的位置是：/etc/sysconfig/docker。
如果是 CentOS6 则添加下面这行：

OPTIONS=--graph="/data/tools/docker"--selinux-enabled -H fd://

 

如果是 Ubuntu 则添加下面这行（因为 Ubuntu 默认没开启 selinux）：

OPTIONS=--graph="/data/tools/docker" -H fd://#

或者

DOCKER_OPTS="-g /data/tools/docker"

 

最后重新启动，Docker 的路径就改成 /data/tools/docker 了。

如果是CentOS7 就是用如下：

修改docker.service文件，使用--graph参数指定存储位置

sudo  vim  /usr/lib/systemd/system/docker.service 

文本内容：ExecStart=/usr/bin/dockerd下面添加如下内容：

​      --graph /data/tools/docker

 

2.修改完成后reload配置文件
sudo systemctl daemon-reload

3.重启docker服务
sudo systemctl  restart docker.service

4.修改默认存储路径的任务已经完成了，期待下一个《非root用户加入docker用户组省去sudo （三）》
备注：如果docker是1.12或以上的版本，可以修改（或新建）daemon.json文件。修改后会立即生效，不需重启docker服务。

sudo  vim  /etc/docker/daemon.json

修改如下：

{"registry-mirrors": ["http://***.***.com"],"graph":"/data/tools/docker"}

 5.希望你能顺利完成操作，有问题尽量还是多看官网文档吧，有惊喜！

\--------------------- 
作者：jwensh 
来源：CSDN 
原文：https://blog.csdn.net/u013948858/article/details/78424115 
版权声明：本文为博主原创文章，转载请附上博文链接！