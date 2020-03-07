# Git操作指南

## 1. 提交已有项目（非从git上pull下的项目）到github

```bash
echo "# JavaDemos" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/linkwanggo/JavaDemos.git
git push -u origin master
```

