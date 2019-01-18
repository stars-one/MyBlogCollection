原文链接：http://www.cnblogs.com/kexing/archive/2018/12/01/10050064.html
### 前言

一般情况，我们在Java中给数组排序，比起自己写个冒泡排序，更加喜欢使用Java中自带的sort方法，也就是`Arrays.sort`方法

但是，这个方法只会将数组从小到大排列，如果我们需要从大到小排列的数组，怎么办呢？

### 思路

我的想法是，把经过`Arrays.sort`方法之后从小到大排列的数组，后面位置的元素与之前的元素进行交换，这样，不就是实现了从大到小的排列了吗？

**需要注意的是：我们得分两种情况，一种是数组中的元素个数是偶数，另外一种则是数组的元素个数为奇数**

下面则是我实现的方法，经过测试，没有错误

	/**
	 *传入一个有序的数组a（从小到大排序），返回一个从大到小的数组
	 * @param a 传入的数组（有序）
	 * @return 返回一个数组（从大到小）
	 */
	public static int[] sort(int[] a){
		int[] temp = a;

		if(temp.length%2==0){
			//数组里面的个数为偶数
			for (int i = 0; i <= temp.length/ 2; i++) {
				int temp1 = a[i];
				temp[i]=temp[temp.length-1-i];
				temp[temp.length - 1-i] = temp1;
			}
		}else{
			//数组里面的个数为奇数
			for (int i = 0; i < temp.length / 2; i++) {
				int temp1 = a[i];
				temp[i]=temp[temp.length-1-i];
				temp[temp.length - 1-i] = temp1;
			}
		}
		return  temp;
	}