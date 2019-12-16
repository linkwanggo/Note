# redis安装和配置

## 1.安装
- 下载安装包
  上次课前资料提供的安装包，或者:[官网下载](https://redis.io/download)

- 解压
```shell
 tar -xvf redis-4.0.9.tar.gz
```

- 编译安装
```shell
 mv redis-4.0.9 redis
 cd redis
 make && make install
```

## 2.配置
修改安装目录下的redis.conf文件
```shell
vim redis.conf
```

复制配置文件

```
cp /home/linkwanggo/software/redis/redis/redis.conf /etc/
```

修改以下配置：

```shell
#bind 127.0.0.1 # 将这行代码注释，监听所有的ip地址，外网可以访问
protected-mode no # 把yes改成no，允许外网访问
daemonize yes # 把no改成yes，后台运行
```

## 3.启动或停止
redis提供了服务端命令和客户端命令：
- redis-server 服务端命令，可以包含以下参数：
  start 启动
  stop 停止
- redis-cli 客户端控制台，包含参数：
  -h xxx 指定服务端地址，缺省值是127.0.0.1
  -p xxx 指定服务端端口，缺省值是6379

## 4.设置开机启动

1) 输入命令，新建文件

```sh
vim /etc/init.d/redis
```

输入下面内容：

```sh
#!/bin/sh  
#  
# redis - this script starts and stops the redis-server daemon  
#  
# chkconfig:   - 85 15  
# description:  Redis is a persistent key-value database  
# processname: redis-server  
# config:      /usr/local/redis-2.4.X/bin/redis-server  
# config:      /usr/local/ /redis-2.4.X/etc/redis.conf  
# Source function library.  
. /etc/rc.d/init.d/functions  
# Source networking configuration.  
. /etc/sysconfig/network  
# Check that networking is up.  
[ "$NETWORKING" = "no" ] && exit 0  
redis="/usr/local/bin/redis-server" 
prog=$(basename $redis)  
REDIS_CONF_FILE="/etc/redis.conf" 
[ -f /etc/sysconfig/redis ] && . /etc/sysconfig/redis  
lockfile=/var/lock/subsys/redis  
start() {  
    [ -x $redis ] || exit 5  
    [ -f $REDIS_CONF_FILE ] || exit 6  
    echo -n $"Starting $prog: "  
    daemon $redis $REDIS_CONF_FILE  
    retval=$?  
    echo  
    [ $retval -eq 0 ] && touch $lockfile  
    return $retval  
}  
stop() {  
    echo -n $"Stopping $prog: "  
    killproc $prog -QUIT  
    retval=$?  
    echo  
    [ $retval -eq 0 ] && rm -f $lockfile  
    return $retval  
}  
restart() {  
    stop  
    start  
}  
reload() {  
    echo -n $"Reloading $prog: "  
    killproc $redis -HUP  
    RETVAL=$?  
    echo  
}  
force_reload() {  
    restart  
}  
rh_status() {  
    status $prog  
}  
rh_status_q() {  
    rh_status >/dev/null 2>&1  
}  
case "$1" in  
    start)  
        rh_status_q && exit 0  
        $1  
        ;;  
    stop)  
        rh_status_q || exit 0  
        $1  
        ;;  
    restart|configtest)  
        $1  
        ;;  
    reload)  
        rh_status_q || exit 7  
        $1  
        ;;  
    force-reload)  
        force_reload  
        ;;  
    status)  
        rh_status  
        ;;  
    condrestart|try-restart)  
        rh_status_q || exit 0  
    ;;  
    *)  
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart| reload|orce-reload}"  
        exit 2  
esac

```

然后保存退出

注意：以下信息需要根据安装目录进行调整：

> EXEC=/usr/local/bin/redis-server # 执行脚本的地址
>
> REDIS_CLI=/usr/local/bin/redis-cli # 客户端执行脚本的地址
>
> PIDFILE=/var/run/redis.pid # 进程id文件地址
>
> CONF="/usr/local/src/redis-3.0.2/redis.conf" #配置文件地址

2）设置权限

```sh
chmod 755 /etc/init.d/redis
```



3）启动测试

```sh
/etc/init.d/redis start
修改权限：chmod 755 /etc/init.d/redis
启动服务：service redis start
停止服务：service redis stop
重启服务：service ngredisnx restart
```

启动成功会提示如下信息：

```
Starting Redis server...
Redis is running...
```



4）设置开机自启动

```sh
chkconfig --add /etc/init.d/redis
chkconfig redis on
```

