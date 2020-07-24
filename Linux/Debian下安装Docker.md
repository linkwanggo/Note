## 安装

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh --mirror Aliyun #调用阿里云镜像
```



## 配置

1. 开放局域网访问

```bash
vim /lib/systemd/system/docker.service
```

修改`[Service]`下的`ExecStart`

```bash
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```

2. 修改Image默认存放路径

```bash
vim /etc/docker/daemon.json
```

```bash
{
    "graph": "/mnt/linkwanggo/docker",  # 默认存放路径
    "insecure-registries": ["192.168.2.111:5000"]  # 如果有registory添加上，没有就删除
}
```

```bash
systemctl daemon-reload
systemctl restart docker
```

