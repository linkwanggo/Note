# Git初始化项目

## 1. 提交已有项目（非从git上pull下的项目）到github

```bash
echo "# JavaDemos" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin (SSH)git:xxx.git
git push -u origin master
```

