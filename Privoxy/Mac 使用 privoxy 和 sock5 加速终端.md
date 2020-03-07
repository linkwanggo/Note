# Mac 使用 privoxy 和 sock5 加速终端

## 1. 安装 privoxy

```
brew install privoxy
```

修改 `/usr/local/etc/privoxy/config`

在结尾添加(端口自行更改)

```
listen-address 0.0.0.0:1087
forward-socks5 / localhost:1188 .
```

第一行的 1087 表示 privoxy 的 http 代理端口，可以自行设置；第二行表示本地 Sock5 客户端的端口，一般都为 1080。

配置好后可以直接使用 brew 自带的启动服务

```
brew services start privoxy
brew services stop privoxy
brew services restart privoxy
```

也可以按需要直接手动启动

```
/usr/local/sbin/privoxy /usr/local/etc/privoxy/config
```

## 2. 添加gfwlist.action

```
pip install gfwlist2privoxy # pip必须为python2
```

参数说明：

- `-i`/`--input` 输入，本地 gfwlist 文件或文件 URL。这里使用上面的 [gfwlist](https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt)
- `-f`/ `--file` 输出，即生成的 action 文件的目录。这里输出到 `/etc/privoxy/gfwlist.action`
- `-p`/ `--proxy` SS 代理地址，生成后可以修改。这里是 `127.0.0.1:1081`
- `-t`/ `--type` 代理类型，生成后也可以修改。这里是 `socks5`
- `--user-rule` 用户自定义规则文件，这个文件中的规则会被追加到 gfwlist 生成的规则后面


```
gfwlist2privoxy -i https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt -f /usr/local/etc/privoxy/gfwlist.action -p 127.0.0.1:1188 -t socks5
```

修改`/usr/local/etc/privoxy/config`

将所有`***.action`和`***.filter`都注释掉

在末尾添加

```
actionsfile gfwlist.action
```

## 3. 配置代理

在～/.zshrc 中添加以下配置，表示 zsh 使用的代理方式。

```
export http_proxy='http://localhost:1087'
export https_proxy='http://localhost:1087'
```

若不希望全局使用代理，可以手动执行命令，这样代理只会在当前终端有效。

或者--添加手动开关的方法

```
# 打开
function proxy_on() {
    export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
    export http_proxy="http://127.0.0.1:8118"
    export https_proxy=$http_proxy
    echo -e "已开启代理"
}
# 关闭
function proxy_off(){
    unset http_proxy
    unset https_proxy
    echo -e "已关闭代理"
}
```

全局配置完成后需要重启终端或刷新配置

```
source ~/.zshrc
```

重启privoxy

```
brew services restart privoxy
```

## 3. 测试

```
curl ip.gs
```

## 4. 若gfwlist不生效

建议刷新配置

```
source ~/.zshrc
```

## ---END---