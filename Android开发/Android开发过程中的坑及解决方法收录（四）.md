原文链接：http://www.cnblogs.com/kexing/archive/2018/12/08/10088879.html
### 1.某个控件要放在Linearlayout布局的底部（底部导航条）

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
	  	...//嵌套的其他布局
	  	<LinearLayout
	  		android:layout_width="match_parent"
	    	
	    	android:layout_height="wrap_content">
	    </LinearLayout>	
	</LinearLayout>    

简单说明一下，上面的代码中有一个Linearlayout，里面嵌套了两个Linearlayout

这里的关键是嵌套里面的第一个`Linearlayout`布局，注意这个布局里面的这两行属性代码

		`android:layout_height="0dp"`    
		`android:Layout_weight="2"`

第二个Linearlayout就是可以放在底部的一个Linearlayout（当然你可以写你自己的布局）

### 2.RecyclerView显示图片卡顿优化

**思路：**图片太多，显示卡顿的原因主要是因为在RecyclerView滑动的过程中同时加载网络的图片，所以卡顿。

我们实现滑动的时候不加载网络图片，当不滑动的时候再加载网络图片，这样流畅度就可以提高许多

1. 在`RecyclerView`的`Adapter（自己写的）`中添加一个判断`RecyclerView`是否滑动的boolean变量`isScrolling`

		protected boolean isScrolling = false;
	    
	    public void setScrolling(boolean scrolling) {
	        isScrolling = scrolling;
	    }
2. 之后在`Adapter`里面的`onBindViewHolder`方法控制加载图片

		@Override
	    public void onBindViewHolder(ViewHolder holder, int position) {
	        String url = mlist.get(position).getImg().getUrl();
	        if (!isScrolling){
	        //我使用的是Ion显示图片框架
	        //如果不在滑动，则加载网络图片
	            Ion.with(holder.imageView.getContext())
	                    .load(url)
	                    .withBitmap()
	                    .placeholder(R.drawable.grey)
	                    .intoImageView(holder.imageView);
	        }else {
	        //如果在滑动，就先加载本地的资源图片
	            Drawable temp = holder.imageView.getResources().getDrawable(R.drawable.grey, null);
	            holder.imageView.setImageDrawable(temp);
	        }
	
	    }

4. 在相应的`Activity`中调用`RecyclerView`的`addOnScrollListener`方法，设置一个滑动监听器

		 mRv.addOnScrollListener(new RecyclerView.OnScrollListener() {
            @Override
            public void onScrollStateChanged(RecyclerView recyclerView, int newState) {
                if (newState == RecyclerView.SCROLL_STATE_IDLE) { // 滚动静止时才加载图片资源，极大提升流畅度
                    adapter.setScrolling(false);
                    adapter.notifyDataSetChanged(); // notify调用后onBindViewHolder会响应调用
                } else{
                    adapter.setScrolling(true);
                }
                super.onScrollStateChanged(recyclerView, newState);
            }
        });
        
### 3.ScrollView与RecyclerView滑动冲突

这里使用`NestedScrollView`即可，然后设置`RecyclerView`的`NestedScrollingEnabled`属性为`false`

两种方法设置`RecyclerView`的`NestedScrollingEnabled`属性

	- 调用`RecyclerView`的`setNestedScrollingEnabled`方法
	- 在xml文件里面，把`RecyclerView`直接设置为`flase`
	
### 判断ScrollView是否滑动到底部

给`ScrollView`添加一个滑动监听器，然后进行相关处理

		 mNestedsv.setOnScrollChangeListener(new NestedScrollView.OnScrollChangeListener() {
            @Override
            public void onScrollChange(NestedScrollView v, int scrollX, int scrollY, int oldScrollX, int oldScrollY) {
                View view = mNestedsv.getChildAt(0);
                if (mNestedsv.getHeight()+mNestedsv.getScrollY() ==view.getHeight()){
					//相关提示
					//相关操作
					//下拉刷新，数据更新操作
					//...                    
                }
            }
        });
        
### 4.使用okhttp返回数据相同解决方法

看了资料，好像是`respone.body().string()`只能调用一次，还有okhttp是有缓存的

使用的情景：有一个API接口，每次访问改接口，都会返回不同的json数据，但是使用okhttp，每次访问该API返回的数据都是相同

**我的解决方法：**

给API请求时添加参数，有些API是可以带参数的，可以修改参数，达到是不同网址的效果

### 5.RecyclerView数据更新

调用`Adapter`的`notifyDataSetChanged`方法即可

**使用需要注意的是，List必须是同一个对象，调用List.addAll方法即可把另外一个同类List里面的全部数据存放进去**