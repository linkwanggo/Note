# Privoxy启动失败如何查找原因

## 查看启动方式

```bash
service privoxy status
```

```json
 privoxy.service - Privacy enhancing HTTP Proxy
   Loaded: loaded (/lib/systemd/system/privoxy.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-06-25 16:21:21 CST; 4min 42s ago
     Docs: man:privoxy(8)
           https://www.privoxy.org/user-manual/
  Process: 875 ExecStart=/usr/sbin/privoxy --pidfile $PIDFILE --user $OWNER $CONFIGFILE (code=exited, status=0/SUCCESS)
 Main PID: 879 (privoxy)
    Tasks: 1 (limit: 4915)
   Memory: 4.6M
      CPU: 178ms
   CGroup: /system.slice/privoxy.service
           └─879 /usr/sbin/privoxy --pidfile /var/run/privoxy.pid --user privoxy /etc/privoxy/config
```

## 非后台启动方式查找原因

```bash
privoxy --no-daemon
```

## ---END---

