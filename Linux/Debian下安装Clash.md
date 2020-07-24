# Debian下安装Clash

## 前置

[Clash RESTAPI](https://clash.gitbook.io/doc/restful-api) 可帮助我们选择节点

[Clash下载链接](https://github.com/Dreamacro/clash/releases/download/v0.20.0/clash-linux-armv8-v0.20.0.gz)

## 安装

```bash
gunzip -d clash-linux-armv8-v0.20.0.gz
```

## 配置

配置路径：`~/.config/clash`

修改`config.yaml`, 没有就创建一个

**注意将`external-controller`改为`0.0.0.0:9090`** 这样才能在其他的主机上访问

```json
# port of HTTP
port: 7890

# port of SOCKS5
socks-port: 7891

# redir port for Linux and macOS
# redir-port: 7892

allow-lan: false

# Rule / Global / Direct (default is Rule)
mode: Global

# set log level to stdout (default is info)
# # info / warning / error / debug / silent
log-level: info

# you can put the static web resource (such as clash-dashboard) to a directory, and clash would serve in `${API}/ui`
# # input is a relative path to the configuration directory or an absolute path
# # external-ui: folder

# Secret for RESTful API (Optional)
# secret: ""

# RESTful API for clash
external-controller: 0.0.0.0:9090

# 将你的clash订阅内容放到这里
proxies:
	  - name: "xxx"
    - type: vmess 
    - server: xxx
    - port: xxx
    - uuid: xxxxxx 
    - alterId: 0 
    - cipher: auto
```

## 选择节点

[Clash Proxy API](https://clash.gitbook.io/doc/restful-api/proxies)

**查看所有节点（GET请求）**

用浏览器访问`http://xxx.xxx.xxx.xxx:9090/proxies`可以查看到当前拥有的节点

**通过API选择节点（PUT请求）**

1. 主机上使用Postman

   向`http://xxx.xxx.xxx.xxx:9090/proxies/GLOBAL`发送 **put**请求，**body**中选择**json**格式的数据

   ```json
   {
   	"name": "xxxxxx" # 节点的名称
   }
   ```

   正确将获得**204**返回码

2. linux上使用curl

```bash
curl  -v -H "content-type: application/json" -X PUT "http://localhost:9090/proxies/GLOBAL" -d '{"name": "xxxxx"}'
```

## 开机启动服务

在`/usr/lib/systemd/system`下创建`clash.service`服务

```bash
# edit and save this file to /usr/lib/systemd/system/clash.service
[Unit]
Description=clash
After=network.target

[Service]
WorkingDirectory=/root/.config/clash
ExecStart={clash所在路径}/clash-linux-armv8-v0.20.0 -d /root/.config/clash

[Install]
WantedBy=multi-user.target
```

## 服务操作方式

```bash
systemctl daemon-reload # 修改xxx.service后调用
systemctl enable clash  # 设为开机启动
systemctl disable clash # 取消开机启动

service clash start # 启动服务
service clash stop  # 关闭服务
service clash restart # 重启服务
```





