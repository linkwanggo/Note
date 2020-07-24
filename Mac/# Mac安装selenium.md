# Mac安装selenium



```bash
☁  ~  pip3 install selenium
☁  ~  pip3 list selenium
Package    Version
---------- -------
pip        18.0
selenium   3.14.0
setuptools 40.2.0
urllib3    1.23
wheel      0.31.1
```

## 安装 ChromeDriver

到[官网](http://chromedriver.chromium.org/)查看最新版本

查看[最新版本](http://chromedriver.chromium.org/downloads)与chrome的匹配

![img](https:////upload-images.jianshu.io/upload_images/1864602-e85ddb9614b63eb2.png?imageMogr2/auto-orient/strip|imageView2/2/w/854/format/webp)

ChromeDriver 2.41

查看chrome版本

![img](https:////upload-images.jianshu.io/upload_images/1864602-b4d0e32fdc66fcbf.png?imageMogr2/auto-orient/strip|imageView2/2/w/727/format/webp)

chrome版本

根据自己的操作系统下载相应安装包

> 可以选择到[淘宝镜像](http://npm.taobao.org/mirrors/chromedriver/)下载

下载后，将安装包加入到环境变量。以mac系统为例，将chromedriver移至/usr/bin目录下即可



```bash
☁  ~  sudo mv ~/Downloads/chromedriver /usr/bin
```

## 验证安装



```bash
☁  ~  chromedriver
Starting ChromeDriver 2.41.578706 (5f725d1b4f0a4acbf5259df887244095596231db) on port 9515
Only local connections are allowed.
```

测试能否调用chrome



```bash
☁  ~  ipython
Python 3.7.0 (default, Aug 22 2018, 15:22:33)
Type 'copyright', 'credits' or 'license' for more information
IPython 6.5.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from selenium import webdriver

In [2]: browser = webdriver.Chrome()

In [3]:
```



作者：it书童
链接：https://www.jianshu.com/p/39716ea15d99
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。