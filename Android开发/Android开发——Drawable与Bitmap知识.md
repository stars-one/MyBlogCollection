原文链接：http://www.cnblogs.com/kexing/archive/2018/08/19/9500685.html
##从资源中获得drawable
`Drawable drawable = getResources().getDrawable(R.drawable.xxx);`
##drawable转换bitmapdrawble
` BitmapDrawable bitmapdrawable = (BitmapDrawable) getResources().getDrawable(R.drawable.xxx);`
##drawble转换为bitmap
	
+ 第一种方式

	实际上，先是把drawable转换为bitmapdrawable，再将bitmapdrawable转换为bitmap,比较简单
	
	` Bitmap bitmap  = bitmapdrawable.getBitmap();`
+ 第二种方式

	需要重新绘图，较为麻烦，不过有些需要重绘，大多数都可以使用第一种，所以，还是推荐第一种（懒癌患者推荐）
	
	
		public static Bitmap drawableToBitmap(Drawable drawable) {   
	        // 取 drawable 的长宽   
	        int w = drawable.getIntrinsicWidth();   
	        int h = drawable.getIntrinsicHeight();   
	        // 取 drawable 的颜色格式   
	        Bitmap.Config config = drawable.getOpacity() != PixelFormat.OPAQUE ? Bitmap.Config.ARGB_8888   
	                : Bitmap.Config.RGB_565;   
	        // 建立对应 bitmap   
	        Bitmap bitmap = Bitmap.createBitmap(w, h, config);   
	        // 建立对应 bitmap 的画布   
	        Canvas canvas = new Canvas(bitmap);   
	        drawable.setBounds(0, 0, w, h);   
	        // 把 drawable 内容画到画布中   
	        drawable.draw(canvas);   
	        return bitmap;
    	}   
    
##bitmap转换为drawable
+ 第一种方式

		Drawable drawable = new BitmapDrawable(bitmap); 

+ 第二种方式

		BitmapDrawable bd= new BitmapDrawable(getResource(), bitmap); 

	PS：bitmapdrawable是drawable的子类，可以直接使用drawable中的方法