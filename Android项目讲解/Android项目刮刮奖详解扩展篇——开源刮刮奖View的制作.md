原文链接：http://www.cnblogs.com/kexing/archive/2018/08/22/9512140.html
[Android项目刮刮奖详解（四）](https://www.cnblogs.com/kexing/p/9513406.html)

##前言
我们已经成功实现了刮刮奖的功能了，本期是扩展篇，我们把这个View直接定义成开源控件，发布到JitPack上，以后有需要也可以直接使用，关于自定义控件的知识，不了解的同学可以看这下面我之前写的这两篇

[Android 自定义控件](http://www.cnblogs.com/kexing/p/7760936.html)

[Android开发——发布第三方库到JitPack上](http://www.cnblogs.com/kexing/p/8779000.html)
##实现

1. **定义属性**
	+ text 文字内容
	+ textColor 文字颜色
	+ textSize 文字大小
	+ paintSize 擦除效果的宽度
	+ messageBackground 中奖图片
	+ cover 遮盖层图片或遮盖层颜色
	+ isDrawText 显示文字或者是图片，默认显示文字
	+ clearFlag 达到多少阈值清除遮盖层，默认60
	
	按照之前那一篇自定义控件来操作，我们建立个atts.xml，在其中定义属性，这里，我们可以将中奖图片和信息层颜色合为一项，遮盖层图片和遮盖层颜色合为一项

		<?xml version="1.0" encoding="utf-8"?>
		<resources>
		    <declare-styleable name="GuaJiangView">
		        <attr name="text" format="string"/>
		        <attr name="textColor" format="color"/>
		        <attr name="textSize" format="integer"/>
		        <attr name="messageBackground" format="color|reference"/>
		        <attr name="cover" format="color|reference"/>
		        <attr name="PaintSize" format="integer"/>
		        <attr name="isDrawText" format="boolean"/>
		        <attr name="clearFlag" format="integer"/>
			</declare-styleable>
		</resources>
		
2. **获得我们定义的属性**

		TypedArray ta = context.obtainStyledAttributes(attrs,R.styleable.GuaJiangView);
        text = ta.getString(R.styleable.GuaJiangView_text);
        textColor = ta.getColor(R.styleable.GuaJiangView_textColor,0);
        textSize = ta.getInteger(R.styleable.GuaJiangView_textSize,16);
        coverDrawable = ta.getDrawable(R.styleable.GuaJiangView_cover);
        isDrawText = ta.getBoolean(R.styleable.GuaJiangView_isDrawText,true);
        clearFlag = ta.getInteger(R.styleable.GuaJiangView_clearFlag,60);
        if (!isDrawText){
            backgroundDrawable = ta.getDrawable(R.styleable.GuaJiangView_messageBackground);
        }
        paintSize = ta.getInteger(R.styleable.GuaJiangView_PaintSize,10);
        ta.recycle();

	这里加了个判断，当用户选择只写中奖信息，我们可以屏蔽掉获取信息层的图片设置，防止出错
3. **修改之前的项目代码**

	开源库，自然是不能像之前项目那般写的那么凌乱，自然是得写上厚厚的注释，将代码重构优化一下，还得考虑到相关的逻辑
	
	这里提一下，attrs中可以使用`\|`来使该属性接收两个属性，最常用的还是背景颜色和背景图片合成一项，例如上面定义的attr中的背景
			
		<attr name="messageBackground" format="color|reference"/>
			
	我们可以通过下面的方法来对这样的属性使用，获得之后转换为bitmap

		/**
	     * drawable转换为bitmap
	     * @param drawable 需要转换的drawble
	     * @param width 宽
	     * @param height 高
	     * @return 返回bitmap
     	*/
	    public Bitmap drawableToBitmap(Drawable drawable, int width, int height) {
	
	        if (drawable instanceof BitmapDrawable) {
	            return ((BitmapDrawable) drawable).getBitmap();
	        }
	        Bitmap bitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
	        Canvas canvas = new Canvas(bitmap);
	        drawable.setBounds(0, 0, canvas.getWidth(), canvas.getHeight());
	        drawable.draw(canvas);
	        return bitmap;
	    }
	    
	其实drawable 它本身有一个 draw方法, 只要我们调用`setBounds`设置范围, 在调用`draw`方法就可以直接画了，上面的**drawable其实已经包含有颜色了**,所以我们直接调用`draw`方法即可在画出一个纯颜色的bitmap
	
	简单地观察，这里与会之前的mCanvas是一样的。
	
	以新建的bitmap作为画板，之后drawable在canvas上作画（实际上是画在了bitmap），之后我们返回这个bitmap使用即可
	
	其他地与之前差不多，大家自己琢磨琢磨吧，最后我会发出完整代码的

4. **上传github以及JitPack**

	这里将不多说什么了，我这两篇写的很清楚了
	+ [Git的简单使用](https://www.cnblogs.com/kexing/p/8344413.html)
	+ [Android开发——发布第三方库到JitPack上](https://www.cnblogs.com/kexing/p/8779000.html)

6. **简单的测试使用**

[GuaJiangViewDemo](https://github.com/Stars-One/GuaJiangViewDemo)
我们按照文档上的方法，导入依赖，使用即可

![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822154915687-2057438815.gif)

##番外——做个美女脱衣
本来想单独出来写一篇的，不过，发现也没有什么新的代码要写，我们直接拿开源库来用即可（需要美女穿衣和脱衣的衣服哦~）

我就准备了二次元少女的衣服，将两张图片放到drawable文件夹之中，我们就可以开始工作了

![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822155924975-973173391.png)

弄好之后，我的android Studio竟然卡死了？！

什么鬼！！好在重启一下软件就好了

激动时刻————

![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822160849994-1949104007.gif)

哈哈，成功，刮刮卡项目的详解就到此结束啦~