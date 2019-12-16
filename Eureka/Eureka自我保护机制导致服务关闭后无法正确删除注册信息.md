# Eureka自我保护机制导致服务关闭后无法正确删除注册信息

## 警告：**EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.**

启动两个client，过了一会，停了其中一个，访问注册中心时，界面上显示了红色粗体警告信息：

查阅了很多资料，终于了解了中间的问题。现将理解整理如下：

Eureka server和client之间每隔30秒会进行一次心跳通信，告诉server，client还活着。由此引出两个名词： 
Renews threshold：server期望在每分钟中收到的心跳次数 
Renews (last min)：上一分钟内收到的心跳次数。

前文说到禁止注册server自己为client，不管server是否禁止，阈值（threshold）是1。client个数为n，阈值为1+2*n（此为一个server且禁止自注册的情况） 
如果是多个server，且开启了自注册，那么就和client一样，是对于其他的server来说就是client，是要*2的

我开了两个server，自注册，相关数据如下 

![](http://img.blog.csdn.net/20170418001242248?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvenpwNDQ4NTYxNjM2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

阈值：1+2*1 
renews： 
1）自注册 2 + 2*1 
2）非自注册：2*1

Eurake有一个配置参数eureka.server.renewalPercentThreshold，定义了renews 和renews threshold的比值，默认值为0.85。当server在15分钟内，比值低于percent，即少了15%的微服务心跳，server会进入自我保护状态，Self-Preservation。在此状态下，server不会删除注册信息，这就有可能导致在调用微服务时，实际上服务并不存在。 
这种保护状态实际上是考虑了client和server之间的心跳是因为网络问题，而非服务本身问题，不能简单的删除注册信息

## stackoverflow上，有人给出的建议是： 
1、在生产上可以开自注册，部署两个server 
2、在本机器上测试的时候，可以把比值调低，比如0.49 
3、或者简单粗暴把自我保护模式关闭

4、 关闭自注册

