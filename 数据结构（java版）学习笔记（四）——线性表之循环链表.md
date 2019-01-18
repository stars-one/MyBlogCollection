原文链接：http://www.cnblogs.com/kexing/archive/2018/08/26/9536510.html
## 单向循环链表
PS：有阴影的结点是头结点

### 概念：
最后一个结点的链域值不为NULL，而是指向头结点
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826104305821-1106597394.png)
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826110055100-359193811.png)
### 特点：

从表中的任意结点出发，都可以找到表中其他结点

### 循环条件

p==h

## 双向链表
### 概念
链表中的每一个结点有两个指针域，一个指向前趋，另外一个指向后继

 ![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826105445893-2114867572.png)
  ![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826105841515-1722536937.png)


## 双向循环链表
### 概念
双向链表中的头结点的前趋指针域指向了尾结点，尾结点的后继指针域指向了头结点，即可称为双向循环链表

### 判空条件

头结点的前趋指针与后继指针指向了本身

### 插入算法

**在p结点前插入新结点**
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826112218906-1235784444.png)

1. s.prior = p.prior;
2. p.prior.next = s;
3. s.next = p;
4. p.prior = s; 

		boolean insert(DulNode h,int i,char x){
			DulNode p,s;
			int j = 1;
			p = h.next;//从第一个结点开始
			//查找第i个结点，当p指向头结点h或p指向第i个结点结束
			while(p!=h||j<i){
				j++;
				p = p.next;
			}
			if(j==i){
				s = new DulNode();//初始化s结点
				s.data =x;//存放数据在s结点
				s.prior = p.prior;
				p.prior.next =s;
				s.next =p;
				p.prior =s;
				return true;
			}else{
				return false;
			}
		}
### 删除算法                                                                                                                              
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826113608170-44483946.png)
1. p.prior.next = p.next;
2. p.next.prior = p.prior;

		boolean delete(DulNode h,int i,char x){
			DulNode p,s;
			int j = 1;
			p = h.next;//从第一个结点开始
			//查找第i个结点，当p指向头结点h或p指向第i个结点结束
			while(p!=h||j<i){
				j++;
				p = p.next;
			}
			if(j==i){
				p.prior.next = p.next;
				p.next.prior = p.prior;
				return true;
			}else{
				return false;
			}
		}