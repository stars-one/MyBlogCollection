原文链接：http://www.cnblogs.com/kexing/archive/2018/12/08/10088761.html
- 定义二维数组

	`int[][] a = new int[4][5];`
	
	**可以不指定列数**
	
	`int[][] a = new int[4][];`
+ 获取行

	`int i = a.length();`
	如果使用第一个例子，这里就是返回4
+ 获取列

	`int i = a[0].length();`使用第一个例子，这里就是返回5
	
+ 定义一个对象数组
	
	Book[] books = new Book[50];
	
	这里的Book是个实体类，之后的用法与一维数组的用法是一样的