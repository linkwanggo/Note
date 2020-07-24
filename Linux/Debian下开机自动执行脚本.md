

# Debian下开机自动执行脚本

## 例子

```bash
vim /usr/lib/systemd/system/rclone.service
```

写入：

```bash
[Unit]
Description = rclone

[Service]
User = root
ExecStart = /usr/bin/rclone mount gdrive: /root/GoogleDrive --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
Restart = on-abort

[Install]
WantedBy = multi-user.target
```

重载daemon，让新的服务文件生效：

```bash
systemctl daemon-reload
```

设置开机启动：

```bash
systemctl enable rclone
```

停止、查看状态可以用：

```bash
systemctl stop rclone 
systemctl status rclone
# service同样可用
service rclone restart
service rclone start
service rclone status
```
查看日志

[参考链接](https://pdf-lib.org/Home/Details/9426)

```bash
# 如果需要查询以前的日志信息
journalctl -u rclone.service
# 仅查看当前引导的日志消息
journalctl -u rclone.service -b
# 对于命名的东西<something>.service
journalctl -u service-name
# 如果是需要查看某段具体时间的日志
journalctl -u service-name.service --since "2019-12-20 20:20:00"
```


## ---END---

