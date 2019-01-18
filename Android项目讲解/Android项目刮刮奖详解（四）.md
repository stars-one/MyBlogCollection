原文链接：http://www.cnblogs.com/kexing/archive/2018/08/21/9513406.html
[Android项目刮刮奖详解（三）](https://www.cnblogs.com/kexing/p/9506159.html)
##前言
上一期我们已经是完成了刮刮卡的基本功能，本期就是给我们的项目增加个功能以及美化一番
##目标
+ 增加功能 用户刮卡刮到一定程度的时候，清除遮盖层
+ 在遮盖层放张图片，增加用户体验
+ 增加一个刮完奖回调监听
##实现
###1.自动消除效果
我们首先来了解一下bitmap的`getPixels`方法

		getPixels(@ColorInt int[] pixels, int offset, int stride,int x, int y, int width, int height) 
>getPixels()函数把一张图片，从指定的偏移位置（offset），指定的位置（x,y）截取指定的宽高（width,height ），把所得图像的每个像素颜色转为int值，存入pixels。
至于参数stride,查了资料，发现看不太懂，于是便是没有继续深究，我们直接用就是了

我们需要一个线程来完成我们的计算像素，因为计算不能一直在UI线程里面执行，可能会出现卡顿，当用户抬起手指的时候，我们就启动这个计算进程来计算用户所擦除的像素点

本功能有些复杂，要想看得懂，需要了解 UI线程更新View的知识和java中进程部分知识，推荐看一下这篇[子进程更新UI](https://www.cnblogs.com/kexing/p/9026960.html)

		private Runnable mRunnable = new Runnable() {
        int[] pixels;

        @Override
        public void run() {

            int w = mBitmap.getWidth();
            int h = mBitmap.getHeight();

            float wipeArea = 0;//擦除像素点计数，初始为0
            float totalArea = w * h;//全部的像素点

            pixels = new int[w * h];
            /**
             * pixels   接收位图颜色值的数组
             * offset   写入到pixels[]中的第一个像素索引值
             * stride   pixels[]中的行间距个数值(必须大于等于位图宽度)。可以为负数
             * x     　从位图中读取的第一个像素的x坐标值。
             * y      从位图中读取的第一个像素的y坐标值
             * width  　　从每一行中读取的像素宽度
             * height 　　　读取的行数
             */
            Bitmap b = mBitmap;
            b.getPixels(pixels, 0, w, 0, 0, w, h);

           //for循环查找用户擦除的像素点，为0则是擦除，wipeArea+1
            for (int i = 0; i < totalArea; i++) {
                if (pixels[i] == 0) {
                    wipeArea++;
                }
            }
			//
            if (wipeArea > 0 && totalArea > 0) {
                int percent = (int) (wipeArea * 100 / totalArea);//计算比例
                if (percent > 50) {
                    isClear = true;//isClear是之前声明的全局变量，
                    postInvalidate();//子进程中调用此方法重绘View
                }
            }

        }
    };

上述代码中有个for循环用来记录擦除像素点，有些疑问，因为鸿洋大神用的不一样，鸿洋大神使用的是下面的嵌套循环

		for (int i = 0; i < w; i++) {
                for (int j = 0; j < h; j++) {
                    int index = i + j * w;
                    if (pixels[index] == 0) {
                        wipeArea++;
                    }
                }
            }

之后我们还需要改写代码，首先，是在触摸事件中增加我们对用户抬起手指的操作

		 case MotionEvent.ACTION_UP:
                new Thread(mRunnable).start();
                break;

之后，通过isClear这个变量来控制是否画出路径，`onDraw`方法之中进行这样的修改

		protected void onDraw(Canvas canvas) {
	        canvas.drawText(message,mBitmap.getWidth()/2-mBackground.width()/2,getMeasuredHeight()/2+mBackground.height()/2,messagePaint);
	        if (!isClear){
	            drawPath();
	            canvas.drawBitmap(mBitmap, 0,0, null);
	        }
    	}
  
当canvas写出了文字，之后就不画遮盖层了，这样便是达到了清除遮盖层的效果

这里需要注意一下**if中的条件**，还要，`isClear`还得用`volatile`修饰，通俗的讲就是加了个锁，防止并发出现错误

###2.遮盖层绘制图片
可能大家对这个功能很不屑，认为自己之前不是会了吗，其实没有那么简单，我自己尝试的时候都出现了错误，经过搜索资料尝试才达到效果。
先说下我遇到的问题
+ 图片只显示一部分
+ 画笔清除不了图片，画的时候出现黑色的笔迹

第一个问题，我们可以通过Bitmap的静态方法来解决

		public static Bitmap createScaledBitmap(Bitmap src, int dstWidth, int dstHeight,boolean filter)
传入一个需要调整大小的bitmap对象，之后，长度和高度，最后一个参数传入true
		background = Bitmap.createScaledBitmap(background,width,height,true);//对bitmap进行缩放

第二个问题，因为我们使用的是双缓冲技术绘图，所以，我们需要将遮盖层的图片先绘制在mBitmap中去
		mBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);//以获得的宽高创建一个32位的bitmap
        mCanvas = new Canvas(mBitmap);
        mCanvas.drawBitamap(background,0,0,null);
                
###3.回调接口
		
		public interface onGuaCompleteListener{
        	void complete();
		}
    	private onGuaCompleteListener mlistener;

    	public void setGuaCompleteListener(onGuaCompleteListener mlistener) {
        	this.mlistener = mlistener;
    	}

之后在`onDraw`方法里添加回调

		protected void onDraw(Canvas canvas) {
	        Log.d(TAG, "onDraw: 画");
	        canvas.drawText(message,mBitmap.getWidth()/2-mBackground.width()/2,getMeasuredHeight()/2+mBackground.height()/2,messagePaint);
	        if (!isClear){
	            drawPath();
	            canvas.drawBitmap(mBitmap, 0,0, null);
	
	        }else if (mlistener!=null){
	            mlistener.complete();
        	}
	    }

当画到百分之60的时候，就会将isClear的值变为true,同时，就会进入到else if中，回调完成刮奖的接口

我们到MainActivity中设置监听器来监听刮刮卡的完成操作，弹出一个Toast或者是对话框，我这里简单起见就直接弹出一个Toast

		GuajiangView mView = (GuajiangView) findViewById(R.id.view);
        mView.setGuaCompleteListener(new GuajiangView.onGuaCompleteListener() {
            @Override
            public void complete() {
                Toast.makeText(MainActivity.this, "hello", Toast.LENGTH_SHORT).show();
            }
        });
        
##测试图
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180821203843686-1920675016.gif)