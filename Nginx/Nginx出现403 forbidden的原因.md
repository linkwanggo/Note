# Nginx出现403 forbidden的原因

首先查看docker中nginx的日志

```bash
docker logs nginx
```

结果显示：
```
192.168.3.16 - - [16/Jan/2020:14:47:11 +0000] "GET / HTTP/1.1" 403 555 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.117 Safari/537.36" "-"
```

很明显是权限问题。

**那么问题来了，到底是哪里的权限出了问题？**

* 由于启动用户和nginx工作用户不一致所致

  ```bash
  vi /etc/nginx/nginx.conf # 修改顶部user为启动用户即可
  ```

* 访问的文件夹权限太低

  ```bash
  chmod 777 data/
  chmod 777 /data/www
  ```

* 该访问路径下没有index.html

* SELinux设置为开启状态（enabled）的原因

```bash
 /usr/sbin/sestatus # 查看SELinux的状态
 vi /etc/selinux/config
 #SELINUX=enforcing
 SELINUX=disabled # 重启生效
```

