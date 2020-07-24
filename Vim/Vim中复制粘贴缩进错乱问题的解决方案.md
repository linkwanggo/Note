# Vim中复制粘贴缩进错乱问题的解决方案

Vim中复制粘贴缩进错乱问题的解决方案

当你把这段缩进优美的代码直接ctrl+c，ctrl+v到Vim的时候，就会出现如下恶心的情况

可以看到，这种直接粘贴的方式会导致代码丢失和缩进错乱等情况。

## 解决方案 

1. 取消自动缩进
   在命令模式下，使用“:set nosmartindent”和“:set noautoindent”取消自动缩进，然后再粘贴即可。完成后再开启自动缩进“:set smartindent”和“:set autoindent”，以上命令都可使用简写，比如“:set si”，可通过Vim的帮助“:help smartindent”查看相应说明。

2. Paste模式
   Vim的编辑模式中，还有一个Paste模式，在该模式下，可将文本原本的粘贴到Vim中，以避免一些格式错误。通过“:set paste”和“:set nopaste”进入和退出该模式。更简便的方式是，在Vim中设置一个进入和退出Paste模式的快捷键，往“~/.vimrc”中添加一行配置“set pastetoggle=<F12>”，这样即可通过F12快速的在Paste模式中切换，当然快捷键在不冲突的前提下可以任意指定，具体如何指定，参考附带的教程链接。

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

## ---END---