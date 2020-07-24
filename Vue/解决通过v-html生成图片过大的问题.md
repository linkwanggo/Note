# 解决通过v-html生成图片无法控制大小的问题

## 1. 面临的问题

我们知道，通常给一个`img`添加一个`width`属性就能够解决img图片过大的问题。

但是今天遇到了一个棘手的问题：

**在Vue中通过`v-html`命令添加的html内容图片无法正常添加`width`属性。**

源代码如下：

```js
<template>
  <div class="article-container">
    <el-col :span="6">left</el-col>
    <el-col :span="12">
      <div class="content">
        <div v-highlight v-html="article.contentHtml"></div>
      </div>
    </el-col>
    <el-col :span="6">right</el-col>
  </div>
</template>
```

css代码如下：

```scss
<style lang="scss" scoped>
.content {
  overflow: auto;
}
.content img {
  width: 100%;
}
</style>
```

**理论上这个css能够解决img图片过大撑出的问题，到底是哪里有问题呢？**

## 2. 解决方案

css中去掉`scoped` 

看来是vue定义的作用域问题

```css
<style lang="scss">
.content {
  overflow: auto;
}
.content img {
  width: 100%;
}
</style>
```

**成功！**

![image-20200410210909653](https://tva1.sinaimg.cn/large/00831rSTly1gdoz8473tzj30w30hs7d9.jpg)

## ---END---