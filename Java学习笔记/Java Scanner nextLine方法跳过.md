原文链接：http://www.cnblogs.com/kexing/archive/2018/12/23/10163694.html
### 问题描述
Scanner使用了nextInt方法的时候，如果接下来要使用nextLine，会获取不到内容

### 原因
因为Scanner读取用户输入数据，是先判断缓冲区是否含有数据，没有则接收用户输入的数据，把用户输入的数据放在缓冲区中读取。

Scanner先获取用户的内容到缓冲区中，调用nextInt方法读取数值，但是缓冲区的内容里其实还含有个`\n`换行符，nextInt方法将数值取出之后，`\n`仍然存在于缓冲区.

之后我们再调用nextLine方法，由于缓冲区中还存在有内容，所以Scanner这个时候直接去到了缓冲区读取,读取到`\n`，直接换行.

于是，Scanner的nextLine方法就跳过，没有获取到用户输入内容.
### 解决方法
在使用完nextInt之后调用nextLine，将缓冲区中的`\n`清除掉

		int i = scanner.nextInt();
		scanner.nextLine();