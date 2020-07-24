# Linux根据名称杀死进程

今天由于修改代码，不小心使得进程无限循环启动，并无限更换pid，导致正常的kill命令无法杀死进程

于是查找到了以下杀死进程的方法

```
ps -ef | grep listener | grep -v grep | awk '{print $2}' | xargs kill -9
```

成功杀死进程

