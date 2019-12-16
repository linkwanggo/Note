# Vim设置缩进为4（默认为8）的方法

## 1. 找到vimrc配置文件的位置，并打开

```shell
sudo vim /etc/vimrc
```

## 2. 添加以下配置

```shell
set smartindent
set tabstop=4
set shiftwidth=4
set expandtab
set softtabstop=4
```

# ---END---