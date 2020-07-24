## CSS中图片如何按照比例自动放大缩小不失真

## 解决方案一

在裁剪图片的时候严格按照格式要求进行裁剪，这样图片能够展示出最完美的效果！

但是这不是我想要的 = = 

## 解决方案二（推荐）

通过设置**width, height, max-width, max-height (需要设置什么根据项目而定)**

例如我的博客项目：设定一个**max-width=100**%就能完美解决问题！

* 不设置的时候是这样的：

![image-20200422213622907](https://tva1.sinaimg.cn/large/007S8ZIlly1ge2vg49b33j30f10fp7ce.jpg)

* 设置**max-width=100**%之后是这样的：

  ![image-20200422213734749](https://tva1.sinaimg.cn/large/007S8ZIlly1ge2vhb2nolj30bm08ngnz.jpg)

怎么样，不错吧！

## ---END---