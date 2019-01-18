原文链接：http://www.cnblogs.com/kexing/archive/2018/08/22/9517867.html
[GuaJiangViewDemo](https://github.com/Stars-One/CirclePointMove)

`欢迎Star`


一个可以简单的刮刮奖View的封装
##测试图
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822154915687-2057438815.gif)
##使用
**1.在根目录上添加**

	maven { url 'https://jitpack.io' }
		
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822143031090-1205396975.png)

**2.添加依赖**

		compile 'com.github.Stars-One:GuaJiangViewDemo:v1.2'
	
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822144425097-744617291.png)

PS:这里因为谷歌更新的缘故，JitPack上是提示用 `implementation` ,但实际上使用的时候，会发生错误，可能是我的android Studio版本过低了，暂时不想去折腾，使用compile也是可以的
 
>api指令完全等同于compile指令，没区别，你将所有的compile改成api，也可以

**3.在布局文件中使用**

![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822144920781-533871503.png)

**4.设置刮奖完成的监听器**
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180822145603405-31856029.png)

##其他

| 属性名字 | 说明 | 默认值 |
| - | :-: | -: |
| text| 文字内容| 无 |
| textSize| 文字大小 | 16 |
| textColor | 文字颜色 | 黑色 |
| PaintSize| 擦除效果宽度 | 10 |
| messageBackground | 信息层背景图片或背景颜色 | 无 |
| isDrawText | 是否显示文字 | true |
|cover | 遮盖层图片或颜色 | 无 |
| clearFlag  |  达到clearFlag阈值清除遮盖层 | 60 |