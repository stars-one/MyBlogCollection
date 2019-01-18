原文链接：http://www.cnblogs.com/kexing/archive/2018/08/20/9502029.html
[Android项目刮刮奖详解（一）](https://www.cnblogs.com/kexing/p/9501933.html)
##前言
上期我们简单地实现了一个画板的功能，用户可以在上面乱写乱画，其实，刮刮奖也是如此，用户刮奖的时候也是乱写乱画的。

##刮刮奖原理
一共有两层画布，底层画布存放中奖信息的图片，上层画布则是一个遮盖层，我们将底层画布成为信息层，上层画布称作为遮盖层。
用户再遮盖层涂画，我们将用户涂画的痕迹从遮盖层擦除，显示出信息层的内容，则就实现了一个简单的刮刮奖。

##实现
基于上期的代码，我们来讲解一下。
上一期中在注释中我也有提示到哪些代码是今天的内容，我们拿来看看吧。
1. **设置背景图**

	首先，我们需要设置信息层的背景图，背景图随意，记得把图片放在drawable文件夹中

		 //背景图
        BitmapDrawable bitmap = (BitmapDrawable) getResources().getDrawable(R.drawable.rewrite6);//从drawable文件夹中获得指定名称的该图片，并转型为bitmapdrawable，R.drawable.xxx
        background = bitmap.getBitmap();//bitmapdrawable通过getBItmap方法得到bitmap
        background = Bitmap.createScaledBitmap(background,width,height,true);//利用Bitmap的静态方法创建一个合适的bitmap（宽高都是之前onMeasure方法中获取的，不太清楚的同学请去上期回顾一下）
		

2. **使用canvas画出背景图**

	**补充 xfermode**
	>Xfermode国外有大神称之为过渡模式，这种翻译比较贴切但恐怕不易理解，大家也可以直接称之为图像混合模式，因为所谓的“过渡”其实就是图像混合的一种，这个方法跟我们上面讲到的setColorFilter蛮相似的。
	
	>查看API文档发现其果然有三个子类：AvoidXfermode, PixelXorXfermode和PorterDuffXfermode，这三个子类实现的功能要比setColorFilter的三个子类复杂得多。
	
	>由于AvoidXfermode, PixelXorXfermode都已经被标注为过时了，所以这次主要研究的是仍然在使用的PorterDuffXfermode：
	 
	 >该类同样有且只有一个含参的构造方法PorterDuffXfermode(PorterDuff.Mode mode)
	 
	 其中的mode有十八种模式，后面谷歌又添加了Add和Overlayl两种模式，下面是十六种模式的图解
	 
	 ![xfermode16种模式图解](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180819212538692-439373127.png)
	 
	 我们怎么去理解这张图呢？我们只要记住一点，先画dst，再画src,有同学说不好记啊，简单，教你个口诀，先画底（dst），再画上（src）
	
	+ 第一个模式clear是清除
	+ 第二个src则是只显示上层图片
	+ 第三个dst则是只显示底层图片
	+ 第四个srcOver如图所示，显示出dst图片的四分之三，显示src的全部
	+ 其他的不多说了，
	
	我们即将用到的是dsc\_out,讲解一下
	
	先画dst，再画src，src消失，只剩下dst，这其实就是橡皮擦的原理，我们利用这个擦除遮盖层就可以显示出信息层中的图片了
	
	
	明白了原理之后，我们来看onDraw方法，在onDraw方法中，使用canvas将背景图画出，这里顺序是先画信息层，之后再到遮盖层，遮盖层将mBitmap直接画出来，回顾一下，这里是使用到了双缓冲技术，canvas直接复制了mBitmap，在View中显示出来，mBitmap其实是mCanvas在上面画出了用户手指的移动痕迹
		canvas.drawBitmap(background,0,0,null);//画出信息层

        canvas.drawBitmap(mBitmap, 0,0, null);//画出遮盖层
        mOutterPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_OUT));//设置xfermode dst\_out
        mCanvas.drawPath(mPath, mOutterPaint);//mCanvas在mBitmap中画出用户的手指的移动痕迹
 
##测试图片
![测试图片](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180819215537404-1605346281.gif)