原文链接：http://www.cnblogs.com/kexing/archive/2018/12/22/10159961.html
### 一、前言  
今天带来的是王牌大作战的破解教程，游戏下载的话，我是直接去TapTap官网下载的 

支付宝内购破解用老套了，今天学点破解的新花样吧！！

### 二、支付宝内购破解  
支付宝的内购破解已经很熟悉了， 直接搜索“9000”，之后找到代码，修改判断条件即可，若不明白，请看我之前写的博客，Android破解之路的学习
  
### 三、Unity3D破解

**PS:破解之前先提醒一下，并不是所有的Unity3D游戏都可以使用本方法进行破解，要满足一个条件**

`是采用Mono打包方式的Unity3D游戏`

#### 判断是否满足条件

1. 判断是否为Unity3D游戏

	首先，如何判断当前的游戏是不是Unity3D游戏，怎么判断呢？
	
	**Androidkiller就是自带有个分析功能**，可以判断当前的游戏是`Unity3D`还是`cocos2D`游戏  
	
	用过Androidkiller软件的朋友都知道，Androidkiller反编译完成之后，就会提示是否分析文件，这个功能就是分析当前的apk使用的游戏引擎是`unity3d`还是`cocos2d`
	
	![](https://attach.52pojie.cn/forum/201812/13/122539kilcnfinwn66ehmm.png)   
	![](https://attach.52pojie.cn/forum/201812/13/122830ikcxyz5zexktzrw5.png)  
	
2. 是否采用Mono打包方式

	我们通过观察`asset`文件夹是否包含有`Assembly-CSharp.dll`这个文件就可以判断是否采用mono方式，有就是采用了mono方式，没有就不是
	
	之后到工程管理器去查看一下是否有**Assembly-CSharp.dll**这个文件  
	![](https://attach.52pojie.cn/forum/201812/13/123015e7t76p9tnrp7rrj2.png)   
	如果有，则确定这游戏是采用**Unity的Mono打包方式的游戏**  
	  
	
#### 使用的软件：

- Androidkiller

- dnSpy

简单介绍一下dnSpy，dnSpy软件可以反编译dll文件，可以修改.net程序，网上都找得到，这里就不放软件链接了。

#### 破解

Unity3D游戏里的获得金币和获得钻石等等的方法，还有相关的游戏资源都是在这**Assembly-CSharp.dll**文件里面了，我们可以使用**dnSpy**软件对dll进行反编译，dll文件原本也是使用C#这个编程语言开发的，属于.net开发  

我下载的dnSpy好像挺新的，我打开的时候提示要下载.net 框架，下载完之后就可以打开了  

1. 反编译`Assembly-CSharp.dll`文件

	打开dnSpy软件，把**Assembly-CSharp.dll**拖进去，展开，我们可以看到有许多的资源  
	![](https://attach.52pojie.cn/forum/201812/13/123957cbfxxlhxl3fh35kd.png)   
  
	这么多，我们也一个个打开的开，难免头大，这时候还是得使用搜索大法  
 
2. 搜索关键字`coin` 

	我们是准备修改金币， 那么直接搜索**coin(按下crtl+shift+k搜索)**  
	![](https://attach.52pojie.cn/forum/201812/13/124106ngaag1znfngdfaag.png)
	
	搜索coin,之后还是有很多结果，我们稍微看一下右边，有个**GameData.Resources,**resources就是资源的意思，可能就在这里面，我们点击进去看看  
	![](https://attach.52pojie.cn/forum/201812/13/124419vq2zccd20ozdiiqk.png)   
	我们可以找到Resource，展开目录，就可以看见Star,Coin这些关键字，其实这些就是金币，星星的数量，里面还有有get_coin方法，字面意思就是获得金币，我们进方法里面瞧瞧  
	![](https://attach.52pojie.cn/forum/201812/13/124604ep9zspjmrjj6j2eq.png)   
	![](https://attach.52pojie.cn/forum/201812/13/124909zm4fmicffkhu84lf.png)   
3. 修改代码  
	
	修改有两种方法，一种是直接右键，选择编辑方法，另外一种则是修改IL指令  
	![](https://attach.52pojie.cn/forum/201812/13/125029hel5ezi99844umux.png)   
  
	开始的时候，我是选择了编辑方法，但是修改之后，点击编译，之后就报错了  
	![](https://attach.52pojie.cn/forum/201812/13/125211fhjthchdplcyhcy0.png)   
	上网一查，了解到，有些dll资源混淆了，无法直接编辑方法
	
	**编辑方法无法修改的话，只能通过IL指令修改了**  
	  
	然后我又去看了IL指令[IL指令集](https://www.52pojie.cn/thread-253232-1-1.html)  
  
	看了好久，才发现我想要的那一条指令  
	Ldc.I4 将所提供的 int32 类型的值作为 int32 推送到计算堆栈上。  
	  
	我们点击IL指令，查看当前方法的IL指令  
	![](https://attach.52pojie.cn/forum/201812/13/125648iddil3qax9z59233.png)   
	  
	**我们使用Ldc.I4 方法，写上10000（之前写9999，进入到游戏只有1000金币），点击Ldc.I4.0，然后会出现下拉菜单，选择Ldc.I4方法**  
	**![](https://attach.52pojie.cn/forum/201812/13/125809hfpgpa0lr6rs0gdl.png) **  
	点击确定之后，我们可以看到代码变了  
	![](https://attach.52pojie.cn/forum/201812/13/130022to2l4xac1lo4oxol.png)   
	改完金币之后，我们还可以修改其他的星星的数目，还有皇冠的数目，修改完毕之后，点击文件菜单，选择保存模块  
	  
	之后，使用Androidkiller删除一下发送短信等垃圾权限，反编译，安装，可以看到我们修改成功了。  
  
### 四、测试截图  
![](https://attach.52pojie.cn/forum/201812/13/130703doi2jianpfwp742p.png)   
  
![](https://attach.52pojie.cn/forum/201812/13/130716h82sgbp1wp2fzgiu.png)   
 
### 下载地址：  
王牌大作战破解版：  
链接: https://pan.baidu.com/s/1y9ad2aBMbmzAwgR5Tm3s1Q 提取码: m82r