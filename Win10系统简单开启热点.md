原文链接：http://www.cnblogs.com/kexing/archive/2018/09/29/9726856.html
## 介绍
笔记本电脑使用的都是无线网卡，我们可以通过这网卡来开启热点供手机使用，说起开热点，大家都是想到的使用360随身wifi或者是猎豹wifi来开启热点吧，我个人不太喜欢使用这些软件，原因因为有DNS拦截，每次手机第一次连接上该Wifi的时候，都会出现个360网站，所以我今天就来使用更加简单的方法来开启热点

## 步骤



1. 使用下面两行命令行即可开启热点（命令行在cmd中输入，如何进入cmd就不用多说了，不会的的同学请问度娘）

	`netsh wlan set hostednetwork mode=allow ssid="输入Wifi名称，双引号忽略" key="输入密码，双引号忽略"`
	
	`netsh wlan start hostednetwork`
	
	**PS：需要注意，要管理员权限**

2. 需要到网络设置中设置网络共享

找到更改适配器

![](https://img2018.cnblogs.com/blog/1210268/201809/1210268-20180929214705958-55903449.png)

我们可以看到，我们开启的热点的Wifi连接

![](https://img2018.cnblogs.com/blog/1210268/201809/1210268-20180929214817270-1780916069.png)

之后选择当前你正在使用的网络，右键属性，选择共享网络，将其共享给那个热点的Wifi连接，确定即可开始愉快地使用wifi了

![](https://img2018.cnblogs.com/blog/1210268/201809/1210268-20180929214955430-1352723300.png)

**PS：这个wifi太久不用就会连接不上网络了，这个时候先把wifi热点给关闭了(代码在下面),之后将网络的共享关闭，之后再按上述操作开启wifi热点，win7系统可能无法使用...**

`netsh wlan stop hostednetwork`

**为了方便，我已经将开启wifi和关闭wifi都打包成了bat文件，右键以管理员权限运行就好，如果要修改wifi名称和密码，参考第一条步骤，用记事本打开开启wifi的那个bat文件即可**

[链接: https://pan.baidu.com/s/1Ih-mAoQ5GW1CTp2rYM4ZmQ 提取码: kcdg](https://pan.baidu.com/s/1Ih-mAoQ5GW1CTp2rYM4ZmQ)