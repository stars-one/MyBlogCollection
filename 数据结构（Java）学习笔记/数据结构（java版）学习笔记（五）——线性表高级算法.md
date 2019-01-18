原文链接：http://www.cnblogs.com/kexing/archive/2018/08/26/9537870.html
## 1.线性表的逆置运算
### 顺序表的逆置
#### 算法说明：
设有一个具有n个元素的线性表存放在一个一维数组A[M]中的前n个数组中，编写一个算法将这个线性表原地逆置
#### 算法要求：
将原表中的第一个元素变为新表中的最后一个元素，原表中的的最后一个元素变成第一个元素，使ai变成ai-1的前趋
#### 算法原理：
以原表的中间为界，将两边的数值进行交换
#### 算法代码
		
		void inverse(int a[],int n){
			int t;
			for(int i=0;i<=(n-1)/2;i++){
				t = a[i];
				a[i]=a[n-1-i];
				a[n-1-i] = t;
			}
		}
### 单链表的逆置
#### 算法说明
在原有的单链表上进行逆置
#### 算法要求
第一个结点变为最后一个结点，最后一个结点变为第一个结点，单链表从最后一个结点开始（头结点不变）
#### 算法原理
从原表的第一个结点开始，在遍历原表时将各结点的指针逆转，最后将头结点指向最后一个结点，即新表的第一个结点
#### 算法代码
		
		class Lnode{
			char data;
			Lnode next;
		}
		boolean inverse(Lnode h){
			Lnode r,q,p;
			p=h.next;
			if(p==null){
				//链表为空，无须反序
				return false;
			}else if(p.next==null){
				//链表上只有一个结点，无须反序
				return false;
			}
			q=p;
			p = p.next;
			q.next =null;
			while(P!=null){
				r = p.next;
				p.next = q;
				q = p;
				p=r;
			}
			h.next = q;//头结点指向第一个结点
			return true;
		}
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826164922425-658118344.png)
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826165249558-2063696605.png)
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826165704628-1993857832.png)
![](https://images2018.cnblogs.com/blog/1210268/201808/1210268-20180826170045771-942563663.png)



## 2.线性表的归并
###算法要求
将两个按元素值递增有序排列的单链表A和B归并为一个按元素值递增有序排列的单链表C
### 有序单链表的归并

		boolean hb(Lnode pa,Lnode pb){
			Lnode pc,p,q;
			pc = new Lnode();
			p = pc;
			while(pa.next!=null&&pb.next!=null){
				pa = pa.next;
				pb = pb.next;
				if(pa.data<pb.data){
					q = pa;
					q.next = pb;
					p.next =q;
				}else{
					q = pb;
					q.next = pa;
					p.next =q;
				}
			}
			while(pa.next!=null){
				pa = pa.next;
				p.next =q;
					
				
			}
			while(pb.next!=null){
			
			}
						
		
		}