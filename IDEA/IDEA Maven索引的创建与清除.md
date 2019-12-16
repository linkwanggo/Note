# IDEA Maven索引的创建与清除

## 索引的创建与更新

打开settings--》Build，Execution--》Build Tools--》Maven--》Repositories

![](https://img2018.cnblogs.com/blog/720750/201809/720750-20180930160820148-168348567.png)

## 索引的删除

这里有两个，上面那个是本地索引，有时候你会发现，自己仓库里明明有jar包了，但是在pom文件中就是不提示，这个时候，就可以选中本地库，然后点击右侧Update就可以啦。如果还不行的话，找到你idea总配置目录，一般是在{home}\.IntelliJIdea\system\Maven\Indices下面,这里的｛home｝是自己配置的，默认是在C:\Users\你的用户名 这个路径下。

![](https://img2018.cnblogs.com/blog/720750/201809/720750-20180930160716296-1866619630.png)

# ---END---