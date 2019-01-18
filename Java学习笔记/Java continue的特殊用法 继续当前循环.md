原文链接：http://www.cnblogs.com/kexing/archive/2018/12/01/10050125.html
### 前言

今天java练习的时候，遇到了一道有趣的题目，加深了我对`cotinue`的理解，所以我写个笔记，记录一下`continue`的特殊用法

### continue作用说明

这里我使用个例子来简单说明一下：

	for(int i=0;i<5;i++){
		if(i==2){
			continue;
		}
		System.out.print(i);
	}

上述代码	最终结果是为`0134` 我们可以理解为忽略掉了`i=2`的这种情况

**问题来了，如果我们想使用`continue`但不想把那种情况给忽略掉，怎么办呢？**

很简单，其实，把在`continue`语句前加上`i--`即可

	for(int i=0;i<5;i++){
		if(i==2){
			i--;
			continue;
		}
		System.out.print(i);
	}
	
### 习题

下面的题目则是我做的练习题

> 题目要求：在1-10范围内，循环生成7个数，并且不重复

		Random random = new Random();
		int[] a = new int[7];
		for (int i = 0; i < a.length; i++) {
			int num = random.nextInt(10)+1;
			boolean flag = false;//是否重复
			//遍历之前数组，如果有重复就置flag为true
			for (int j = 0; j <a.length; j++) {
				if (num==a[j]){
					flag=true;//存在了重复的数字
					break;
				}
			}
			if (flag){
				//因为使用continue语句的时候，会忽略当前的i，从而执行i++，
				// 所以我们在执行continue语句之前，让i的值减1，
				// 这样就能使得下次循环还是停留在原本的i
				i--;
				continue;//存在重复了的数字，重新循环，获取随机数
			}else{
				a[i] =num;//不存在重复，则赋值
			}
		}
		for (int i = 0; i < a.length; i++) {
			System.out.print(a[i]+" ");
		}