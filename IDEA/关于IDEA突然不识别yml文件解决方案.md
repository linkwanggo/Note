# 关于IDEA突然不识别yml文件解决方案

之前idea还能识别yml文件，突然不能识别了原因是：
在settings ---> File Types的Text选项中添加application.yml，导致application.yml被识别为txt文件

解决方案： 删除 Text下的application.yml选项即可

![](https://img-blog.csdnimg.cn/20190116180911197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg2NDMzNg==,size_16,color_FFFFFF,t_70)

![](https://i.loli.net/2019/11/17/9qzodRQgs6PJWI4.png)