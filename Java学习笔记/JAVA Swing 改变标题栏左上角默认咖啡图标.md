原文链接：http://www.cnblogs.com/kexing/archive/2018/11/10/9938599.html
## 前言

最近使用Java的swing开发了一个小程序，想要实现改变标题栏左上角的图标，找了网上的资料，经过了一个下午的尝试，都是未能成功，最后，终于是在Java的一本书上找到了结果

**我只能说，网上的东西真的坑**

## 实现

		Image image = Toolkit.getDefaultToolkit().getImage(jframe.getClass().getResource("文件路径"));
		jframe.setIconImage(image);
		
## 路径的坑

`Image`是`AWT`

`getImage`是`toolkit`的一个方法，参数接收一个`URL`

调用`Jframe`的`setIconImage()`方法即可设置图标

这里需要注意路径的问题

![](https://img2018.cnblogs.com/blog/1210268/201811/1210268-20181110103909984-1316179279.png)

**其他情况以此类推**