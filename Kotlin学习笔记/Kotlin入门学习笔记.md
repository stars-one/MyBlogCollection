原文链接：http://www.cnblogs.com/kexing/archive/2018/10/18/9812676.html
## 前言

*本文适合人群 有一定的java基础*
## 变量与方法
- 变量声明及赋值

	`var 变量名: 变量类型`
	
	`val 变量名： 变量类型`
	
	**这里，var表示可以改变的变量，val则是不可改变的变量（第一个赋值之后，之后都无法改变此变量的数值）**
**PS:在kotlin中，类型首字母都是要大写的,还有，冒号之后有空格**

- 变量声明及赋值
	
	var name =""
	
	var age= 1
	
	val name= ""
	
	...
	
	
**赋值的话，直接赋值就好，系统会自动的给变量定义类型**

- 变量声明特殊例子

	name: String?
	
	**声明一个String类型的变量name,name可以为null,这里是空指针防护，在后面会讲到**
	
	

- 方法声明格式

	fun 方法名(){

	}
	
PS：默认为public,返回值为空

	fun helloworld() {
		print("hello world")
	}

**无需使用分号，print省略了Java中的System.Out**
**和java一样，println换行，print不换行**	

	fun helloworld(): String{
		print("hello world")
	}
	
**返回一个String**	

	protected fun helloworld(): String{
		print("hello")
	}
**声明方法为protected，除此之外，还有private,internal**

## 类
- 类的声明

	class Student{
		var name = ""
		var age = 0
	}
	
**这里是写了一个Student类，我们上面虽然只有两行代码，但实际上将其转为java代码时，其实是包含了set和get方法**	

	class Student{
			var name = ""
				set(value){
					field = ...//复写set方法
				}
				get(value){
					...
				}
			var age= 0
		}
	
- companion 伴生方法 相当于java中的静态方法,得在

- init 主构造方法

- 实现接口与继承

- 直接构造方法
	class Student(var name: String,var age: Int){
	
	}
	
**可以直接通过参数创建一个Student对象**

- 创建对象

	`var s = Student("Zhangsan",15)
## 继承父类及实现接口

	class Student: school,Person(){
		...
	}

## 循环
for(i in 0..100) 0到100 都取值

for(i in 0 until 100)

for(i in list)

遍历list

## swich分支
	val result = ""
	when(result){
		"OK","SUCCESS" -> print("成功")
		"Falied" -> {
			一系列操作...
		}
		else ->{
			一系列操作...
		}
	} 

**result如果是OK或者是SUCCESS,执行输出成功，如果是Falied,执行后面的操作，都不符合，则执行else后面定义的操作**

如果判断是否是某个类的实例，使用`is`关键字

	val student = Student("Zhangsan",15)
	when(result){
		is Student -> print("")
		else ->{
			一系列操作...
		}
	}
 
## 空指针防护
	 
- ?. ?

	fun get(name: String?): Int{
		return name?.length ?: 0 //如果name为null，则返回0,
	}

- ?. 表示前面的变量可能为null
	
		fun toUpperCase(content: String?){
			println(content?.toUpperCase)  //如果content为null，则不执行
		}

视频下载地址：

郭霖—快速上手kotlin链接：https://pan.baidu.com/s/1eo6B8EFdjWpwc-j8yI9iFw 
提取码：fvmq	

[菜鸟kotlin教程]( http://www.runoob.com/kotlin/kotlin-tutorial.html)

**本篇其实也是笔记，可能有不准确的地方，多多包涵**