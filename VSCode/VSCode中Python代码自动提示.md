# VSCode中Python代码自动提示

自己写的模块，VSCode中无法自动提示。可以按下面步骤试试：

## 1. 添加模块路径(可忽略)

文件 – 设置 – 首选项，搜索autoComplete，点击"在settings.json中编辑"，添加模块路径

```
  "python.autoComplete.extraPaths": [
      "C:\\Module\\Lib\\site-packages",
      "C:\\Module\\Scripts",
```

## 2. python.jediEnabled

文件 – 设置 – 首选项，搜索python.jediEnabled，去掉前面的勾，使用Microsoft python analysis engine

