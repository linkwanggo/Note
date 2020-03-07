# 安装zsh及on-my-zsh

示例使用Debian环境 其他环境只需稍作修改

## 1. 安装zsh

```
apt install zsh
```

切换zsh为默认bash

```
chsh -s /bin/zsh
```

查看当前shell

```
cat /etc/shells
echo $SHELL
```

重新打开ssh,发现bash已切换到zsh

`将~/.bashrc中的配置拷贝到~/.zshrc`

## 2. 安装on-my-zsh

```
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

或

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 3. 安装插件

插件只要安装到oh-my-zsh特定的插件目录下 就会被自动发现

### 3.1 自动提示必不可少（[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)）

```
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

### 3.2 语法高亮 （[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)）

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

### 3.3 autojump ([autojump](https://github.com/wting/autojump))

```
apt install autojump
```

修改  ~/.zshrc

添加 `. /usr/share/autojump/autojump.sh` 其他系统请到github上查看

刷新配置

```
source ~/.zshrc
```

只要我们进过一个路径 就会被记录

```
j [name] # 进入目录
```

## ---END---

