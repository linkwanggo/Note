# 如何设计Header

设计简洁明了，是我的原则

仿照：[速盘官网](https://www.baidusu.com/)

## Header预览

![](https://i.loli.net/2019/10/24/Liv93cGwe2pZ5Qy.png)

主体使用了 flex布局，自动分为左右结构，非常符合预期。

## html代码

```vue
<template lang="html">
  <div class="header-wrapper">
    <div class="header">
      <router-link
        class="logo-link"
        to="/"
      >
        <img class="logo" src="@/assets/logo/favicon.png" alt="">
        <span class="title">linkwanggo</span>
      </router-link>
      <div class="menu-link">
        <span class="menu-item">音乐</span>
        <span class="menu-item">音乐</span>
        <span class="menu-item">音乐</span>
      </div>
    </div>
  </div>
</template>
```

## CSS代码

```scss
.header-wrapper {
  position: relative;
  width: 100%;
  height: 80px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0,.1);

  .header {
    position: relative;
    display: flex;
    width: 100%;
    height: 80px;
    line-height: 80px;
    margin: 0 auto 30px;
    z-index: 100;
    box-sizing: border-box;
    align-items: center;
    justify-content: space-between;
    padding: 0 120px;
      
    .logo-link {
      display: flex;
      align-items: center;
      .logo {
        width: 60px;
      }
      .title {
        color: #333;
        font-weight: 700;
        font-size: 24px;
        margin-left: 20px;
      }
    }
    .menu-link {
      display: flex;
      align-items: center;
      .menu-item {
        color: #666;
        padding: 0 22px;
      }
    }
  }
```

**---END---**