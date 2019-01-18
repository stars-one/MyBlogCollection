原文链接：http://www.cnblogs.com/kexing/archive/2018/12/08/10088778.html
### 某个控件要放在Linearlayout布局的底部

	<LinearLayout
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:layout_width="match_parent"
	    android:orientation="vertical"
	    android:layout_height="match_parent"
	    ...>
	    <LinearLayout
	    	android:layout_width="match_parent"
	    	android:orientation="vertical"
	    	android:layout_height="0dp"
	    	android:Layout_weight="2">
	  	...//嵌套的其他布局……
	  	</LinearLayout>
	  	<LinearLayout
	  		android:layout_width="match_parent"
	    	
	    	android:layout_height="wrap_content">
	    </LinearLayout>	
	</LinearLayout>    

简单说明一下，上面的代码中有一个Linearlayout，里面嵌套了两个Linearlayout，第二个Linearlayout就是