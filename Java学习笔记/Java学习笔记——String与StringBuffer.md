原文链接：http://www.cnblogs.com/kexing/archive/2018/12/16/10125705.html
### String常用方法
- spilt 

	切割，返回一个String数组

- charAt 

	取得String中的一个字符，返回该字符

- toCharArray 

	将String转换为char数组
	
- equals 

	比较
	
- equlalsIgnoreCase 

	忽略大小写的比较
	
- indexOf 

	从左往右检索
	
- lastIndexOf

	从右往左检索

- substring

	从index截取字符串，第二个参数不包括那个index

- concat

	连接两个String，也可以直接+号也可以连接两个字符串

- trim

	删除String中的空格，换行字符

- startsWith

	是否以xx开头

- endsWith

	是否以xx结尾

### StringBuffer常用方法

- append

	原来的内容+参数内容，修改原来的内容

	`StringBuffer  s = new StringBuffer("hello");`

	`s.append("world!");`

	**s里面的内容为helloworld!**

### String与StringBuffer

String str = new String("abc")

创建了几个对象？
2个

解释：

"abc"放在常量池（在常量池中创建了一个对象）
new String也创建了一个对象

String str = new String("bc)+"a;//创建了3个对象


String 内容不改变，只是在常量池中创建了新的对象，之后指针指向新的对象

StringBuffer 可以改变内容