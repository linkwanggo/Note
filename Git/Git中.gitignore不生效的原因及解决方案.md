

[摘抄自：Git忽略规则(.gitignore配置）不生效原因和解决](https://www.cnblogs.com/rainbowk/p/10932322.html)]

## 原因
**在添加.gitignore之前已经被git进行版本管理**

因为在git忽略目录中，新建的文件在git中会有缓存，如果某些文件已经被纳入了版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的。

## 解决方案一（可用）

这时候我们就应该先把本地缓存删除，然后再进行git的提交，这样就不会出现忽略的文件了。

```bash
git rm -rf --cached .
git add .
git commit -m 'update .gitignore'
git push -u origin master
```

需要特别注意的是：

* .gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
* 想要.gitignore起作用，必须要在这些文件不在暂存区中才可以，.gitignore文件只是忽略没有被staged(cached)文件，
  对于已经被staged文件，加入ignore文件时一定要先从staged移除，才可以忽略。

## 解决方案二(未尝试)

在每个clone下来的仓库中手动设置不要检查特定文件的更改情况。

```
git update-index --assume-unchanged PATH         //在PATH处输入要忽略的文件
```

## 补充

**在使用.gitignore文件后如何删除远程仓库中以前上传的此类文件而保留本地文件**
在使用git和github的时候，之前没有写.gitignore文件，就上传了一些没有必要的文件，在添加了.gitignore文件后，就想删除远程仓库中的文件却想保存本地的文件。这时候**不可以直接使用"git rm directory"**，这样会删除本地仓库的文件。可以使用"**git rm -r –cached directory**"来删除缓冲，然后进行"**commit**"和"**push**"，这样会发现远程仓库中的不必要文件就被删除了，以后可以直接使用"**git add -A**"来添加修改的内容，上传的文件就会受到.gitignore文件的内容约束。

额外说明：**git库所在的文件夹中的文件大致有4种状态**

**Untracked:**

未跟踪, 此文件在文件夹中, 但并没有加入到git库, 不参与版本控制. 通过git add 状态变为Staged.

**Unmodify:**
文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这种类型的文件有两种去处, 如果它被修改,
而变为Modified. 如果使用git ``rm``移出版本库, 则成为Untracked文件

**Modified:**
文件已修改, 仅仅是修改, 并没有进行其他的操作. 这个文件也有两个去处, 通过git add可进入暂存staged状态,
使用git checkout 则丢弃修改过, 返回到unmodify状态, 这个git checkout即从库中取出文件, 覆盖当前修改

**Staged:**
暂存状态. 执行git commit则将修改同步到库中, 这时库中的文件和本地文件又变为一致, 文件为Unmodify状态.
执行git reset HEAD filename取消暂存, 文件状态为Modified

**Git 状态 untracked 和 not staged的区别**
1）untrack   表示是新文件，没有被add过，是为跟踪的意思。
2）not staged 表示add过的文件，即跟踪文件，再次修改没有add，就是没有暂存的意思