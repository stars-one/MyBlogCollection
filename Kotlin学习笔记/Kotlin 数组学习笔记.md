原文链接：http://www.cnblogs.com/kexing/archive/2018/12/01/10049930.html
### 创建数组
- 初始值为空的String数组
	
	`val arrayEmpty = emptyArray<String>()`
	
- 长度为5，初始值为空的Int数组		

	`val arrayEmpty = emptyArray<Int>(5)`

- 长度为5，初始值为0的Int数组	

	`val array4 = Array(5, {0})`

- 使用闭包创建数组，x的平方，i从0开始 数组存放为0,1,4,9,16		

	`val array = Array(4, { i -> i * i }) ` 

### 遍历数组

- 普通遍历
		
		for(item in array){
			println(item)
		}

- forEach遍历

		array.forEach {
		    println(it)
		}

- 遍历数组下标
		
		for (item in array.indices) {
		    println(item)
		}