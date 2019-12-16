# Vim中复制粘贴缩进错乱问题的解决方案

Vim中复制粘贴缩进错乱问题的解决方案

当你把这段缩进优美的代码直接ctrl+c，ctrl+v到Vim的时候，就会出现如下恶心的情况

可以看到，这种直接粘贴的方式会导致代码丢失和缩进错乱等情况。

解决方案 
vim进入paste模式，命令如下：

```bash
:set paste
```

进入paste模式之后，再按i进入插入模式，进行复制、粘贴就很正常了。 
命令模式下，输入

```
:set nopaste
```

解除paste模式。

paste模式主要帮我们做了如下事情：

textwidth设置为0 
wrapmargin设置为0 
set noai 
set nosi 
softtabstop设置为0 
revins重置 
ruler重置 
showmatch重置 
formatoptions使用空值

# ---END---