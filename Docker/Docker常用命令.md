# Docker常用命令

[Docker镜像仓库](https://hub.docker.com/)

**友情提示：注意防火墙开启相应的端口**

## 搜索镜像

```bash
docker search mariadb
```

## 下载镜像

```bash
docker pull mariadb:{TAG} 
```

## 查看镜像

```bash
docker images
```

## 删除镜像

```bash
docker rmi {IMAGE ID}
```

## 创建容器

```bash
docker run --name mariadb -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d arm64v8/mariadb:10.1
```

| 名称 | 含义                                                  |
| ---- | ----------------------------------------------------- |
| -p   | 端口映射 {宿主机}:{容器} 将容器中的端口映射到宿主机上 |
| -d   | 作为守护线程运行                                      |
| -i   | 与容器进行交互式操作                                  |
| -t   | tty 开启一个交互终端                                  |

## 启动&停止&重启容器

```bash
docker start mariadb
docker stop mariadb
docker restart mariadb
```

## 查看容器

```bash
docker ps
docker ps -a
```

## 删除容器

```bash
docker rm {CONTAINER ID}
```

## 进入容器

```
docker exec -it mysql bash # 普通版进入方式
docker exec -it redis /bin/sh # alpine版进入方式 
```

## 查看容器日志

```bash
docker logs mysql
```

## 从宿主机拷贝到容器

```bash
docker cp {宿主机文件路径} {容器名}:{容器内部路径}
docker cp /home/sda1/data/html nginx:/root 
```

## 容器开机自启动

[参考链接](https://blog.csdn.net/xtjatswc/article/details/86586769)

在使用docker run启动容器时，使用--restart参数来设置：

```bash
docker run -m 512m --memory-swap 1G -it -p 58080:8080 --restart=always   
--name bvrfis --volumes-from logdata mytomcat:4.0 /root/run.sh
```

如果创建时未指定 --restart=always ,可通过update 命令设置

```
docker update --restart=always 容器名称 
```

还可以在使用on - failure策略时，指定Docker将尝试重新启动容器的最大次数。默认情况下，Docker将尝试永远重新启动容器。

```bash
docker run --restart=on-failure:10 redis  
```



