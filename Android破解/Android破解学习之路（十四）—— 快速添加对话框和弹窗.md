原文链接：http://www.cnblogs.com/kexing/archive/2018/11/05/9908467.html
##工具介绍

一个集合弹窗和对话框的smali文件，结合Androidkiller使用，简单地在APP中插入弹窗或对话框

##工具使用

1. 使用前把Cracker.smali文件放入需要弹窗的那个Activity的同目录

2. 之后调用代码即可显示弹窗或者是对话框

3. 调用代码及说明

	调用代码放在`setContentView`之后即可
	
	- 显示弹窗
	
		const-string v0, "hello" #内容，修改即可
		
    	invoke-static {p0, v0}, Lcom/wan/zhifubao/Craker;->showToast(Landroid/content/Context;Ljava/lang/String;)V
    
	- 显示标题和内容的对话框
	
		const-string v0, "hello" #标题

    	const-string v1,"message" # 内容
    	
    	invoke-static {p0, v0,v1}, Lcom/wan/zhifubao/Craker;->showDialog1(Landroid/content/Context;Ljava/lang/String;Ljava/lang/String;)V
    
	
	- 显示标题，内容和确定按钮的对话框
	
		const-string v0, "title" #对话框的标题
		
	    const-string v1, "message" #对话框的内容
	    
	    const-string v2, "\u786e\u5b9a" # 对话框确定按钮的文字
	    
	    invoke-static {p0, v0, v1, v2}, Lcom/wan/zhifubao/Craker;->showDiaglog2(Landroid/content/Context;Ljava/lang/String;Ljava/lang/String;Ljava/lang/String;)V
	    
	- 显示加入QQ群的对话框