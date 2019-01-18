原文链接：http://www.cnblogs.com/kexing/archive/2019/01/01/10205282.html
**可能你只想简单的使用，暂时不想了解太多的知识，那么请看这里，了解一下如何读文件，写文件**
## 读文件示例代码
		File file = new File("D:\\test\\t.txt");
		//这里的有两条斜杠，其实实际的路径为D:\test\t.txt,多的那一条斜杠是转义用的
		InputStreamReader inputStreamReader = new InputStreamReader(new FileInputStream(file), "GBK");
		//利用之前的File对象，创建了一个`FileInputStream`对象，之后作为InputStreamReader的构造方法参数传入
		//第二个参数是编码，一般使用GBK或者是UTF-8
		BufferedReader reader = new BufferedReader(inputStreamReader);
		//把InputStreamReader作为参数构造一个BufferedReader
		String s;
		while((s=reader.readLine())!=null){
			System.out.println(s);//循环，将该文件里面的内容全部打印出来
		}
		

## 写文件示例代码
与上面的类似，解释就不多写啦

		File file = new File("Q:\\t.txt");
		OutputStreamWriter outputStreamWriter = new OutputStreamWriter(new FileOutputStream(file), "GBK");
		BufferedWriter writer = new BufferedWriter(outputStreamWriter);
		writer.write("hello world");
		writer.close();//必须要调用close才能成功地将内容写入文件

文件不存在的话会自动生成该文件，**但是如果是带有文件夹的话，就得考虑到文件夹不存在的情况**

例如：

		File file = new File("Q:\\test\\t.txt");
		File temp = file.getParentFile();
		if (!temp.exists()) {
			temp.mkdirs();//文件夹不存在，则新建一个文件夹
		}
		OutputStreamWriter outputStreamWriter = new OutputStreamWriter(new FileOutputStream(file), "GBK");
		BufferedWriter writer = new BufferedWriter(outputStreamWriter);
		writer.write("hello world");
		writer.close();
		
## 复制文件
这里使用的是字节流，把t.txt复制到Q盘中的MY文件夹中，并改名

		FileInputStream inputStream = new FileInputStream("Q:\\t.txt");
		FileOutputStream outputStream = new FileOutputStream("Q:\\MY\\t1.txt");
		int c;
		//读到尾部则是返回-1
		while ((c = inputStream.read()) != -1) {
			outputStream.write(c);
		}
		inputStream.close();
		outputStream.close();

**PS：**这里和之前一样，**如果文件夹不存在**，就得调用mkdirs方法进行创建文件夹的操作，除了txt文件，**其他的文件也是可以这样复制**
## File类（文件类）

File对象代表磁盘中实际存在的文件和目录

静态属性
File.separator 分隔符 windows操作系统是`\`,linux操作系统则是`/`

构造方法
- **File(File parent, String child)** 通过给定的父抽象路径名和子路径名字符串创建一个新的File实例
- **File(String pathname)** 通过将给定路径名字符串转换成抽象路径名来创建一个新 File 实例。

常用方法
- **public String getName()** 返回由此抽象路径名表示的文件或目录的名称。
- **public String getParent()** 返回此抽象路径名的父路径名的路径名字符串，如果此路径名没有指定父目录，则返回 null
- **public File getParentFile()** 返回此抽象路径名的父路径名的抽象路径名的file对象，如果此路径名没有指定父目录，则返回 null
- **public String getPath()** 将此抽象路径名转换为一个路径名字符串
- **public boolean exists()** 判断此抽象路径名表示的文件或目录是否存在
- **public boolean isDirectory()** 测试此抽象路径名表示的文件是否是一个目录
- **public boolean isFile()** 测试此抽象路径名表示的文件是否是一个标准文件。
- **public boolean delete()** 删除此抽象路径名表示的文件或目录
- **public boolean mkdirs()** 创建此抽象路径名指定的目录，包括创建必需但不存在的父目录。
- **public boolean renameTo(File dest)** 重新命名此抽象路径名表示的文件

## IO流笔记
![](https://img2018.cnblogs.com/blog/1210268/201901/1210268-20190101155930708-930307144.png)

主要的还是两种，**字节流**和 **字符流**

所有的文件在硬盘或在传输时都是以字节的方式进行，包括图片等，而字符是只有在内存中才会形成，所以在开发中，字节流使用较为广泛。
### 字节流

#### 1.InputStream(输入)	

- FileInputStream（主要）
- ByteArrayInputStream
- ObjectInputStream
- PidedinputStream
#### 2.OuputStream（输出）
- FileOutputStream（主要）
- ByteArrayOutputStream
- ObjectOutputStream
- PidedOutputStream

---

### 字符流

#### 1.Reader(输入)
- FileReader
- CharArrayReader
- PipedReader
- StringReader
#### 2.Writer（输出）
- FileWriter
- CharArrayWriter
- PipedWriter
- StringWriter

---

### 缓冲流
上面字节流和字符流一般不会直接使用，实际上，缓冲流是一个装饰类，可以给字符流和字节流添加缓冲这功能。

BufferReader
BufferWriter
其实这两个也是包括在字符流里面的