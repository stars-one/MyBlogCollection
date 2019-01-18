原文链接：http://www.cnblogs.com/kexing/archive/2018/11/30/10042986.html
### 普通for循环

	for(i in 1..4){
		println(i)
	}
*结果为1234* **循环四次**



### 反序for循环

	for(i in 4 downTo 1){
		println(i)
	}
*结果为4321* **循环四次**



### until的for循环

	for(i in 1 until 4){
		println(i)
	}
*结果为123* **循环三次**


### 特殊的for循环（指定步长）

	for(i in 1..4 step 2){
		println(i)
	}
*结果为13（步长为2，相当于每次循环i=i+2）*