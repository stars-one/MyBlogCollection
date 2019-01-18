原文链接：http://www.cnblogs.com/kexing/archive/2018/08/19/9501933.html
##前言
最近正在学鸿洋大大的刮刮奖，感觉学有所得，便是来写篇详解（尽管网上有很多了，不过毕竟是自己写的，自己以后方便复习），正文开始
##目标
实现画板功能
##思路
我们需要自定义View来实现画板功能，之后再将其稍微改造即可。
关于自定义View，如果没有了解的同学建议先去了解一下，百度自定义View就会有许多相关教程了，
我在这里也就简单的提一下，自定义View常用的三大类，Paint（画笔），Path（路径），
Canvas(画布)，[三大类方法介绍](https://www.cnblogs.com/kexing/p/9497804.html)

1. **继承View，实现构造方法**

	四个构造方法，我们主要实现两个参数的构造方法即可
		 
		 
	    private Paint mOutterPaint = new Paint(); // 绘制线条的Paint,即用户手指绘制Path
	    private Path mPath = new Path();//记录用户绘制的Path
	    private Canvas mCanvas;//画布，可以画东西
	    private Bitmap mBitmap;//画布在此图片上画画
	    private int mLastX;//x坐标
	    private int mLastY;//y坐标
	    private Bitmap background;//这个是背景图，我们先不理
		public GuaGuaKa(Context context) {
        	super(context);
    	}

	    public GuaGuaKa(Context context, @Nullable AttributeSet attrs) {
	        super(context, attrs);
	        mPath = new Path();
	
	    }

	    public GuaGuaKa(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
	        super(context, attrs, defStyleAttr);
	    }

	    public GuaGuaKa(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
	        super(context, attrs, defStyleAttr, defStyleRes);
	    }
    
		
3. **复写onMeasure方法**

	我们首先获得View的宽高，然后以此的宽高创建一个画布，同时，对画笔进行一些设置
	
		@Override
    	protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
	        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
	        Log.d(TAG, "onMeasure: 测量");
	        int width = getMeasuredWidth();
	        int height = getMeasuredHeight();
	        // 初始化bitmap
	        mBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);//以获得的宽高创建一个32位的bitmap
	        mCanvas = new Canvas(mBitmap);//以bitmap创建了一个画布
	        mCanvas.drawColor(Color.GREEN);//设置画布的颜色为绿色
	        //背景图,下一节解析此部分代码，先注释掉
	        /*BitmapDrawable bitmap = (BitmapDrawable) getResources().getDrawable(R.drawable.rewrite6);
	        background = bitmap.getBitmap();
	        background = Bitmap.createScaledBitmap(background,width,height,true);*/
	        // 设置画笔
	        mOutterPaint.setColor(Color.BLUE);
	        mOutterPaint.setAntiAlias(true);//使用抗锯齿功能，会消耗较大资源，绘制图形速度会变慢
	        mOutterPaint.setDither(true);//图像抖动处理,会使绘制出来的图片颜色更加平滑和饱满，图像更加清晰
	        mOutterPaint.setStyle(Paint.Style.STROKE);
	        mOutterPaint.setStrokeJoin(Paint.Join.ROUND);//圆角，平滑
	        mOutterPaint.setStrokeCap(Paint.Cap.ROUND); //圆角
	        mOutterPaint.setStrokeWidth(20); // 设置画笔宽度
		}	
4. **复写onTouchEvent方法**

	我们在这里复写，处理一下用户的触摸事件当手指按到屏幕上的时候，Path路径之中就使用moveto方法，移动到手指当前位置，
	invalidate刷新View，回调onDraw方法，（还没有画出来）。之后，手指移动，action处于是处于ACTION\_MOVE的状态，Path路径使用lineto方法（画直线），同时，将x，y坐标进行了更新，invalidate刷新View，回调onDraw方法，canvas通过drawpath使用画笔将path画了出来，之后如果用户没有抬起手指，则继续循环ACTION_MOVE中的步骤

		@Override
	    public boolean onTouchEvent(MotionEvent event) {
            //当手指按到屏幕上的时候，Path路径之中就使用moveto方法，移动到手指当前位置，invalidate刷新View,回调onDraw方法,（还没有画出来）,canvas就将画笔移动到那个坐标
            //之后，手指移动，action是处于ACTION_MOVE的状态，Path路径使用lineto方法（画直线），
            // 同时，将x，y坐标进行了更新，invalidate刷新View,回调onDraw方法，canvas通过drawpath使用画笔将path画了条出来（画直线），之后如果用户没有抬起手指，则继续循环ACTION_MOVE中的步骤

            int action = event.getAction();
            int x = (int) event.getX();//获得x坐标
            int y = (int) event.getY();//获得y坐标
            switch (action){
            	case MotionEvent.ACTION_DOWN:
	                mLastX = x; 
	                mLastY = y; 
	                mPath.moveTo(mLastX, mLastY);//之后回调onDraw方法canvas将path
	                break;
            	case MotionEvent.ACTION_MOVE:
	                mPath.lineTo(x, y);//之后回调onDraw方法时canvas画直线到（x，y）该点
	                mLastX = x;//更新x坐标
	                mLastY = y;//更新y坐标
	                break;
            	default:break;
	        }
	        invalidate();//刷新View，回调onDraw方法
	        Log.d(TAG, "onTouchEvent: invalidate");
	        return true;
    }
3. **复写onDraw方法**

	之后我们复写`onDraw`方法，在这里`canvas`用画笔开始画画
	
	
		
		@Override
	    protected void onDraw(Canvas canvas) {
		    Log.d(TAG, "onDraw: 画");
		    //canvas.drawBitmap(background,0,0,null);//下一节解析，这里先注释掉
		    //mOutterPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_OUT));//下一节解析，注释掉
		    mCanvas.drawPath(mPath, mOutterPaint);//mCanvas是我们定义的画布，用户的每次的手指轨迹都被path记录下来，之后mcanvas就使用画笔将用户的手指轨迹画了出来
		    canvas.drawBitmap(mBitmap, 0,0, null);//将mBitmap画出来
	    }

` PS：这里测试的时候发现mCanvas与canvas的顺序可以换个位置，影响不大`
![测试动态图](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180819173845682-428358315.gif)

##简单的流程图
![流程图](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180819154854674-518255936.png)
##补充 双缓冲技术
事实上，刮刮卡这个项目用到了双缓冲技术
> 双缓冲即在内存中创建一个与屏幕绘图区域一致的对象，先将图形绘制到内存中的这个对象上，再一次性将这个对象上的图形拷贝到屏幕上，这样能大大加快绘图的速度。

我们定义了一个mBitmap，之后使用这个MBitmap建立了一个画布，实际上我们的mCanvas只在mBitmap上画画

		mBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);//以获得的宽高创建一个32位的bitmap
	    mCanvas = new Canvas(mBitmap);//以bitmap创建了一个画布
	    mCanvas.drawColor(Color.GREEN);
	    
mCanvas负责将用户的手指痕迹通过`drawpath`方法画出来

		mCanvas.drawPath(mPath, mOutterPaint);

之后，onDraw方法里面的canvas则将MBitmap复制并显示出来
		
		canvas.drawBitmap(mBitmap, 0,0, null);//将mBitmap画出来（将MBitmap直接复制到View上显示）