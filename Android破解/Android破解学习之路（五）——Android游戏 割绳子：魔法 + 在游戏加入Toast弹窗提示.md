原文链接：http://www.cnblogs.com/kexing/archive/2018/01/01/8151182.html

**前言**：这一期的破解教程，有新的知识内容出现啦！

这一期破解的游戏是找不到之前的关键字，怎么破解呢？

破解成功之后，添加一个Toast弹窗提示由XX破解，这操作该如何实现呢？请往下看~

链接: https://pan.baidu.com/s/1dF8jKdF 密码: 6666

破解步骤：
-----

### 1.试玩，找不到关键字

由图片可以看出，这是中国移动的咪咕游戏，这款游戏是接入了咪咕游戏基地的sdk，如果你不懂sdk，那么我也不会解释，百度是你的好老师！

游戏使用了咪咕游戏基地的sdk，其中的smail文件一般都有Billing这个关键字，那么我们试着搜索看看

### 2.查找Paycallback关键字

![](http://images2017.cnblogs.com/blog/1210268/201712/1210268-20171230193654867-1006195388.png)

 可以看到，我们经过文件类型筛选之后，还是有许多的结果，这时我们可以再原来的关键字上再加上个pay进行搜索

![](http://images2017.cnblogs.com/blog/1210268/201712/1210268-20171230194004351-910385337.png)

嗯，这下子结果就是少了很多了，咪咕游戏这类sdk，关于支付的类名一般都是含有callback，所以我们可以定位到图中的三个含有callback的文件，第一个我就不圈出来了

 我们一个个使用工具去查看其的java伪代码，则可以再次精确定位到ChinaBillingPayCallBack$1这一个文件

### 3.理解支付逻辑

我们查看ChinaBillingPayCallBack$1这个smail文件的伪代码

 ![](http://images2017.cnblogs.com/blog/1210268/201712/1210268-20171230194550617-181432731.png)

可以看到，是if与else if的嵌套的分支语句，每个判断都是给i赋值，估计大家看得不太懂，说实话，其实我也不太懂，那个for循环里面有个setResultCode，其中的参数为i，个人觉得这个应该是支付成功的关键代码，问题就来了，我们不知道i等于几时，这个setResultCode才是支付成功，这个时候，我们有什么办法吗？答案很简单，一个个地试，我们将i的值改变，测试APP，就是可以知道i为1的时候，就是会进入支付成功的方法

![](http://images2017.cnblogs.com/blog/1210268/201712/1210268-20171230200553335-48319156.png)

回到smail文件中，与之前的java伪代码对比，可以发现v1就是那个i值，之后，就好办了，我们跳过所有的判断语句，修改v1的最后的值进行测试

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101143218362-1861266366.png)

PS：const/4 v1,0x1 这个的意思为给v1赋值为1，如果是const/4 v1,0x2，就是给v1赋值为2，我这里只是简单的说明，想要深入的可以百度搜索一下smail中const指令

我们测试到1，就会发现支付已经成功了，这也算是成功破解了，不过，我觉得还是得再分析分析这smail文件

### 4.破解思路

 我们分析一下上述的smail文件，首先，v1接收参数p1的值，之后，判断参数p1与v2的值，相等的话也就是v1为1，之后向下执行，从前面我们知道，v1为1的时候，就是支付成功的代码，我们可以删除掉这一行的判断从而实现破解的功能，不相等的话就是跳转到cond\_1，记住末尾的cond\_0和goto_0这两个，之后会提到  

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101144456799-247196872.png)

 我们从上面跳转到了cond\_1这里，v2就是被赋值为3了，之后，参数p1又是接着与v2比较，相等则是往下执行，也就是给v1赋值为3，跳转到goto\_0,也就是之前上面说的地方，不相等就是跳转到cond_2，以此类推，大家应该能理清楚大概逻辑了吧

PS：大家可能不知道if-ne这个语句的作用，可以直接使用鼠标放在上面，之后就会浮出提示

 ![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101145329893-495240961.png)

两种方法破解：

上面都有提及到了，这里写个总结

1.删除第一个判断语句，不跳转到之后去判断，,v1就是为1

2.在所有判断结束之后，给v1赋值为1

### 5.增加Toast提示

接下来就是新学的内容了，重点来了！！

首先，先确定开始的界面是哪一个activity，我使用了当前activity 这款APP，之后运行游戏，左上角就是会有提示，我们可以知道CTRMActivity这个  

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101151921706-269832350.jpg)

第二步，我们需要找到这个activity对应的smail文件，直接搜索（如果你不嫌麻烦的话，可以找包名，依次地寻找）

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101152300799-1365998079.png)

我们在a文件夹那里，右击，之后选择打开方式->打开文件路径

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101152454424-1743772399.png)

第四步，新建一个txt格式文件，修改文件名为craker，之后把扩展名改为smali（呃，好像发现我之前都是写成了smail，大家注意下啊，我就不修改了）

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101152826940-1792020151.png)

返回到Androidkiller中，点击右边的刷新按钮

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101152919331-8211706.png)

之后，你就发现有craker这个smali文件了，打开它，写上下列代码，可以看到，有个toast的字符串，我们只需要将这里修改成我们想要Toast提示的信息,如果是中文的话，需要转换成Unicode，直接使用Androidkiller自带的工具转换吧，前面几篇也是有说过，我就不多说了

```smali
.class public Lcrack;
.super Ljava/lang/Object;
.source "craker.java" .method public static toast(Landroid/content/Context;)V
    .locals 2 .prologue const-string v0, "by stars-one"
 
        const/4 v1, 0x1 invoke-static {p0, v0, v1}, Landroid/widget/Toast;->makeText(Landroid/content/Context;Ljava/lang/CharSequence;I)Landroid/widget/Toast;
 
        move-result-object v0
 
        invoke-virtual {v0}, Landroid/widget/Toast;->show()V return-void .end method

```

 最后，我们在CTRMactivity中的onCreate方法里写上调用的代码，就大功告成了

		invoke-static {p0}, Lcrack;->toast(Landroid/content/Context;)V 

诶，等等，onCreate方法呢？没有？？擦嘞，不可能。。

好吧，我们找错了，我们再运行一遍游戏，我看到了一个ChannelSplash，就决定是它了，去Androidkiller找找看（下面的图片是我已经修改成功了的图片，请忽略弹出的Toast）

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101161939112-1716110129.jpg)

 这次找对了，这里有`onCreate`方法，之后的步骤与上面的一样，这里我就不多说了，注意一下最后调用代码添加的位置，还有记得删除掉之前在CTRMactivity中添加的代码

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101162255268-1734263245.png)

成功了，不过想到了`activity`的模式，好像`onstart`方法也是可以用，刚才的那个`CTRMactivity`就是有一个`onstart`方法，我把前面的删除，之后按照之前的步骤，尝试在CTRMactivity中里面写上了调用的那一段代码

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101182315112-1116570912.png)

之后，测试的时候，发现成功显示

 ![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101182724518-1803191400.jpg)

###  6.删除权限，测试

我们可以把这个APP的名字更改了，这样显得比较高端哈！

直接搜索割绳子就行了，之后进入到`string`文件，修改`app_name`  

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101184541174-72253320.png)

这里不得吐槽一下，这个游戏好像删除发送短信会出错，进入游戏的时候提示应用已停止，暂时还不知道解决，有哪位大神看到这里能知道如何解决这个问题，希望能够告知~~感激不尽！！

还有，悬浮窗广告还不知道怎么破解，有大神能教教我吗~~

![](http://images2017.cnblogs.com/blog/1210268/201801/1210268-20180101184107971-1442294741.jpg)