原文链接：http://www.cnblogs.com/kexing/archive/2018/12/23/10163707.html
### instanceof

对象名 instanceof 类名

该对象是否属于该类

		Animal animal = new Dog();
		if(animal instanceof Dog){
			Dog d = (Dog)animal;
		}

### 多态

		Animal animal = new Dog();
		animal.sleep();//先调用子方法，如果子方法未重写（复写），则执行父类中的该方法

### 抽象类
子类继承父类，父类是个抽象类，子类必须实现父类中的抽象方法，如果不想实现，可以将子类定义为抽象类，让下一个继承子类的类来实现
### 接口
接口里面只能存放常量和抽象方法

定义的变量会默认添加`public` `static` `final`这些关键字

方法也是会自动添加`abstract`

例如：
`int numer = 10;` 其实相当于 `public static final int numer = 10;`

`publlic void hello();` 相当于 `public abstract void hello();`

**接口可以实现多继承**

**接口也可以实现多态**

		Eat eat = new Dog();
		eat.print(Eat eat);//狗实现了吃的接口，实现了吃接口里面的print方法，之后调用eat.print()，执行的是狗实现的print方法

		void print(Eat eat){
			eat.print();
		}