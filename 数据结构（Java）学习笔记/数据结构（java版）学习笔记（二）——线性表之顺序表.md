原文链接：http://www.cnblogs.com/kexing/archive/2018/08/05/9387708.html

顺序表的优点：
-------

1.  随机存取元素方便，根据定位公式容易确定表中每个元素的存储位置，所以要指定第i个结点很方便
2.  简单，直观

顺序表的缺点：
-------

1.  插入和删除结点困难
2.  扩展不灵活，难以确定分配的空间
3.  容易造成浪费

顺序表的实现：
-------

这里我简单说一下吧，Sqlist类实现了ListIntf接口，也就是我们上一节中所提到的接口，之后eclipse中就会提示我们复写ListIntf中的方法，我们根据顺序表的特点，逐一复写即可

详情看注释吧~

**PS：所有的参数i都是序号，从1开始**

```
import java.util.Scanner;

public class SqList implements ListIntf{
    final int maxlen = 1000; //线性表中可容纳最多数据元素数目
    String v[] = new String[maxlen];
    int len =0;//len是长度，1开始，是位序号
    /**
     * 
     * @return 返回线性表中可容纳最多数据元素数目
     */
    int getmaxlen(){
        return maxlen;
    }
    public SqList(){
        //构造方法中实现相关的接收数据操作
        Scanner sc = new Scanner(System.in);
        int n;
        String[] a = new String[maxlen];
        System.out.println("请输入需要的数据个数：");
        n=sc.nextInt();
        sc.close();
        for(int i=0;i<n;i++){
            a[i] = "a"+i;//a[0]的数值是 a0,a[1]的数值是a1，以此类推……
            
        }
        setData(a);
    }
    /**
     * 将String数组a赋值给String数组v
     * @param String数组
     */
    
    public void setData(String[] a){
        v = a;
    }
    @Override
    public int size() {
        return len;
    }

    @Override
    public void clear() {
        v = new String[maxlen];
    }

    @Override
    public boolean isEmpty() {
        return len==0;
    }

    @Override
    public String get(int i) {
        return v[i-1];
    }

    @Override
    public int indexOf(String s) {
        int i;
        for(i=0;i<len;i++){
            if(v[i].equals(s)){
                return i+1;
            }
        }
        System.out.println("顺序表中不存在该元素！！");
        return 0;
    }

    @Override
    public String getPre(String s) {
        //当前是顺序表，是顺序存储结构，无需实现此方法
        return null;
    }

    @Override
    public String getNext(String s) {
        //当前是顺序表，是顺序存储结构，无需实现此方法
        return null;
    }

    @Override
    public void insertElementAt(String s, int i) {
        //首先判断顺序表是否已满，其次判断i是否合法，之后再进行插入操作
        if(len==maxlen){
            System.out.println("顺序表已满！");
            return;//结束判断，跳出判断语句
        }else{
            if(i<1||i>len){
                System.out.println("输入的序号不合法！");
                return;
            }else{
                for(int j=len-1;j>=i-1;j--){
                    v[j+1]=v[j];
                }
                v[i-1] =s;  
                len++;
                return;
            }
        }
    }

    @Override
    public String remove(int i) {

        String string;
        if(i<1||i>len){
            
            System.out.println("输入序号不合法！");
            return null;
        }else{
            string = v[i-1];
            for(int j=i-1;j<len;j++){
                
                v[j-1] = v[j];
            }
            len--;
            return string;
        }
        
    }

    @Override
    public String remove(String s) {
        // TODO Auto-generated method stub
        String string;
        for(int i=0;i<len;i++){
            if(v[i].equals(s)){
                string = v[i];
                remove(i+1);//这里的i是索引（下标）,索引（下标）加1成为序号
                return string;
            }
        }
        System.err.println("当前顺序表中没有该元素");
        return null;
    }
    
}
```