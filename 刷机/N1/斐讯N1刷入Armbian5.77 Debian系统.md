# 斐讯N1刷入Armbian5.77 Debian系统

[参考教程1](https://luotianyi.vc/1306.html)

[参考教程2](https://blog.csdn.net/jeansliu/article/details/103659832)

[刷机资源](https://cnone.lty.fun/home/%E5%B7%A5%E5%85%B7%E5%BA%93/N1)

## 1. 全新的N1需要先降级

### 1.1 打开N1的adb

将N1正常开机，使用HDMI线连接到显示器，USB连接鼠标。

进入主界面后连接鼠标，用鼠标左键单击四下【固件版本】即可开启ADB。（注意不要多次点击：adb开启后再次点击四下会关闭！！！）

![](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-01-23_14-24-08.jpg)

### 1.2 使用webpad的降级工具进行系统降级

打开`资源`中下载的【降级工具】。

解压至任意摸得到的地方，双击运行【onekey】文件夹下的run.bat运行脚本。

`N1的IP地址可以在屏幕中看，也可以在路由器中查看`

![](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-01-23_14-34-09.jpg)

选择[2]回车，随后会让你输入IP地址，之前的主页面截图上自己看就是了……填进去，如果操作正确会出现下图

![](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-02-13_09-12-20.png)

如果连接失败的话，请检查ADB是否已经开启。按照提示进行后，盒子会自动重启，至此降级过程就结束了。

## 2. 制作Armbian的U盘启动器 

### 2.1 将镜像写入U盘

选择`Armbian_5.77_xxx.img`，解压获得一个.img映像文件，下载工具包中的【usbit.zip】，直接运行Usb Image Tool.exe即可

![](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-01-23_14-50-16.jpg)

（注意此操作会格式化U盘）将一个8G以上的U盘插入电脑，在左侧选择U盘，然后再点击Restore，然后选择刚才解压出来的.img镜像，一路yes就会开始写入U盘，等着写入结束即可

### 2.2 修改dtb文件

[下载地址](https://cnone.lty.fun/show/%E5%B7%A5%E5%85%B7%E5%BA%93/N1/dtb%E8%A1%A5%E4%B8%81/meson-gxl-s905d-phicomm-n1-xiangsm.dtb)

*dtb：各种品牌的盒子千千万，每个盒子使用的网卡啊、cpu芯片啊的型号千千万，armbian内核为了能够和这些外设正常工作，就要求提供一种叫做dtb的描述文件，我理解就是针对各个硬件的驱动程序。*

- 为了让斐讯N1的各个硬件可以被armbian正常调度，所以需要给斐讯N1适配一套dtb文件。*
- *dtb文件需要随着内核编译，所以不同armbian内核版本必须使用配套的dtb文件。*
- *目前斐讯N1的dtb文件已经被armbian收录到官方源码库里，但是使用的时候linux负载会显示的很高，所以热心网友为armbian5.77编译了一个fix过的dtb文件。*

#### 2.2.1 将meson-gxl-s905d-phicomm-n1-xiangsm.dtb文件放到dtb/meson-gxl-s905d-phicomm-n1-xiangsm.dtb下面

![](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-01-23_14-57-02.jpg)

#### 2.2.2 修改一下uEnv.ini文件：

dtb_name=/dtb/meson-gxl-s905d-phicomm-n1-xiangsm.dtb

![](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-01-23_15-15-09.jpg)

## 3. 设置N1默认引导为U盘启动

进入刚才的【降级工具】/data目录下（就是包含adb.exe那个文件夹）在此目录打开gitbash

**严重警告：未重启时不要先插入U盘，一定要在执行下面语句结束后，看到N1刚刚开始重启时再插入U盘**

```
.\adb.exe connect 192.168.xx.xx（N1的IP）
.\adb.exe shell reboot update
```

操作正确后，盒子会自动重启并从U盘启动

## 4. 将系统写入N1的eMMC内置存储

正常情况下，使用HDMI线连接N1后，系统已经进入了debian。

或者使用路由器查看N1的IP，用SSH工具进行连接。

默认用户名：root

默认密码：1234

进入后输入当前默认密码，重新设置新密码。

设置完又会提示加新用户，直接【Ctrl+C】跳过，重新连接。CentOS登录则直接完成，如果修改密码请用passwd指令。

**最后提醒：一定要将U盘引导文件中的替换成上面提供的`meson-gxl-s905d-phicomm-n1-xiangsm.dtb`**

执行命令：

`nand-sata-install`

系统将自动写入N1内置的eMMC，完成后poweroff关机，拔掉U盘，重启即可。

# ---END---

