原文链接：http://www.cnblogs.com/kexing/archive/2018/12/26/10179229.html
### 功能
可以根据使用路径修改文件名，已经测试，可以成功运行

### 思路

先是读取到txt文本文件，之后使用String的`spilt`进行分割，每一行的格式为 `旧名字 新名字`，中间的空格可以使用`|`或者其他字符代替，以此为标志分割String

之后将旧名字当做key，新名字当做value写入到map中去

获得文件的所在的文件夹，`listFile`遍历得到所有的文件，之后`getName`获得文件名（这里获得到的文件名是包括有扩展名的）

再次使用String的`spilt`进行处理（参数为"\\."，需要转义），得到文件名和扩展名

`reName`改名字，参数为一个文件对象

### 代码
先把代码贴出来吧，之后再做个有界面的工具~

		class Test {
		
			private static Map<String, String> map;
		
			public static void main(String[] args) {
				map = readTxtFile("T:\\游戏资源\\仙剑4\\音乐目录.txt");
				reName("T:\\游戏资源\\仙剑4\\仙剑奇侠传四音乐");
		
			}
		
			/**
			 *
			 * @param s 文件所在文件夹路径名
			 */
			public static  void reName(String s){
				File file = new File(s);
				if (file.isDirectory()){
					File[] files = file.listFiles();
					for (int i = 0; i < files.length; i++) {
						String h = files[i].getName();
						String[] temp = h.split("\\.");
						String newName = map.get(temp[0]);
						files[i].renameTo(new File(s+"\\"+newName+"."+temp[temp.length-1]));
					}
				}
			}
			private  static HashMap<String, String> readTxtFile(String filePath){
				HashMap<String, String> map = new HashMap<>();
				try {
					String encoding="GBK";
					File file=new File(filePath);
					if(file.isFile() && file.exists()){ //判断文件是否存在
						InputStreamReader read = new InputStreamReader(
								new FileInputStream(file),encoding);//考虑到编码格式
						BufferedReader bufferedReader = new BufferedReader(read);
						String lineTxt = null;
						while((lineTxt = bufferedReader.readLine()) != null){
							String[] split = lineTxt.split(" ");
							map.put(split[0],split[1]);
						}
						read.close();
					}else{
						System.out.println("找不到指定的文件");
					}
				} catch (Exception e) {
					System.out.println("读取文件内容出错");
					e.printStackTrace();
				}
				return map;
			}
		}