# Git错误补救案例

## 1. git commit之后，想要撤回或修改

```bash
git reset --soft HEAD^ # 撤回到上一次commit
git reset --soft HEAD~1 # 等价于上方操作
git reset --soft HEAD~2 # 撤回到2次之前commit的操作
```

**关于`--xxx`的参数说明：**
**--mixed** 

意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 

操作这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

**--soft**  

不删除工作空间改动代码，撤销commit，不撤销git add . 

**--hard**

删除工作空间改动代码，撤销commit，撤销git add . 

注意完成这个操作后，就恢复到了上一次的commit状态。

**顺便说一下，如果commit注释写错了，只是想改一下注释，只需要：**

git commit --amend

此时会进入默认vim编辑器，修改注释完毕后保存就好了。