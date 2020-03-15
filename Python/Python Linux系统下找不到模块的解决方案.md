# Python Linux系统下找不到模块的解决方案

## 原因分析：

pycharm会默认将当前项目路径添加到sys中，因此项目可以正常运行

但当我们手动调用时，当前项目并不在python的环境变量中，因此项目无法正常运行

## 解决方案

在 ~/.zshrc中加入该项目的环境变量

```
export PYTHONPATH=$PYTHONPATH:/home/sda1/script_project/xxxxx/
```

```
source ~/.zshrc
```

## ---END---

