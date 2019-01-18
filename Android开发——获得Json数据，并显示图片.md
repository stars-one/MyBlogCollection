原文链接：http://www.cnblogs.com/kexing/archive/2018/12/09/10091933.html
### 流程介绍

1. 使用`okhttp`网络框架进行` get`请求，获得`json`数据
		
		//一个封装好的工具类的静态方法
		public static void sendOkHttpRequest(final String address, final okhttp3.Callback callback) {
	        OkHttpClient client = new OkHttpClient();
	        CacheControl control =new CacheControl.Builder().build();
	        Request request = new Request.Builder()
	                .cacheControl(control)
	                .url(address)
	                .build();
	        client.newCall(request).enqueue(callback);
	
	    }

	之后我们调用这个方法可以访问网站，并获得返回的数据

		HttpUtil.sendOkHttpRequest("http://www.baidu.com" + limit, new Callback() {
        	@Override
            public void onFailure(Call call, IOException e) {
                Toast.makeText(MainActivity.this, "网络错误", Toast.LENGTH_SHORT).show();
            }
            @Override
            public void onResponse(Call call, Response response) throws IOException {
				//获得返回的数据（按照我的例子，访问百度，返回来的数据其实就是html文件里面的内容
				//如果是其他网站，就是返回其定义返回的数据类型）
                String result = response.body().string();
                //调用GSON框架解析json数据，处理完毕后返回一个该相关类的List
                List<Data.ResBean> mlist = HttpUtil.parseJSONWithGSON(result);
				//之后对返回的数据进行处理或者是调用
				mlist.get(1).getXXX();//相关属性的调用
            }
        });
2. 使用`GSONFormat`插件将`json`数据抽象为实体类（插件自动生成）

	去Android Studio里面搜索GSONFormat插件，安装重启之后，写一个类，然后直接按下`alt+Ins`,选择GSONFormat,之后输入json数据，就可以获得一个对应的实体类了
	
	![](https://img2018.cnblogs.com/blog/1210268/201812/1210268-20181209162843891-1170832863.png)

	![](https://img2018.cnblogs.com/blog/1210268/201812/1210268-20181209162925994-569124511.png)

	
2. 使用`GSON`框架，解析json数据，获得实体类

	下面的方法可以根据自己的需要写

		 /**
	     * 调用GSON解析json数据
	     * @param jsonData json数据
	     * @return 返回图片实体类list
	     */
	    public static List<Data.ResBean> parseJSONWithGSON(String jsonData) {
	        //使用轻量级的Gson解析得到的json
	        Gson gson = new Gson();
	        Data data = gson.fromJson(jsonData, Data.class);
	        List<Data.ResBean> mlist = data.getRes();
	        return mlist;
	    }
    
3. 调用所需要的属性即可

	对象调用get方法即可获得相关的属性，自己需要什么就调用什么，这里就不多说了。
	
4. 使用`Glide`等图片框架加载网络图片

		Glide.with(context).load(url).into(imageView);
		        
	我使用的是另外一款Ion显示图片框架，因为之前使用Glide有些bug，第一次可以加载，但刷新数据之后就无法显示了，可能是因为我使用的Glide3.0版本吧，然后觉得Glide4.0版本使用有些懵，就选择了Ion,感觉和Glide差不多，之前的那个bug也是得以解决，就没有想太多了
	
			 Ion.with(holder.imageView.getContext())
	                    .load(url)
	                    .withBitmap()
	                    .placeholder(R.drawable.grey)
	                    .intoImageView(holder.imageView);