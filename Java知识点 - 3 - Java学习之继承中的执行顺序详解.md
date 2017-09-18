#Java学习之继承中的执行顺序详解

代码块(理解)

(1)用{}括起来的代码。

(2)分类：

A:局部代码块

用于限定变量的生命周期，及早释放，提高内存利用率。

B:构造代码块

把多个构造方法中相同的代码可以放到这里，每个构造方法执行前，首先执行构造代码块。

C:静态代码块

static{}对类的数据进行初始化，仅仅只执行一次。

(3)静态代码块,构造代码块,构造方法的顺序问题?

静态代码块 > 构造代码块 > 构造方法

	class Student {  
   		 static {  
        	System.out.println("Student 静态代码块");  
    	}  
      
    	{  
        	System.out.println("Student 构造代码块");  
    	}  
      
    	public Student() {  
        	System.out.println("Student 构造方法");  
    	}  
	}  
  
	class StudentDemo {  
    	static {  
        	System.out.println("studentDemo静态代码块");  
    	}  
      
    	public static void main(String[] args) {  
        	System.out.println("我是main方法");  
        	Student s1 = new Student();  
        	Student s2 = new Student();  
    	}  
	}  

输出结果：


    <span style="font-family: Arial, Helvetica, sans-serif;">studentDemo静态代码块</span> 
	我是main方法 
	Student 静态代码块 
	Student 构造代码块 
	Student 构造方法 
	Student 构造代码块 
	Student 构造方法 


有继承情况：

	class Son extends Father {  
    	public Son() {  
        	//super();  
        	System.out.println("Son的无参构造方法");  
    	}  
      
    	public Son(String name) {  
        	//super();  
        	System.out.println("Son的带参构造方法");  
    	}  
	}     
  
	class ExtendsDemo6 {  
    	public static void main(String[] args) {  
        	//创建对象  
        	Son s = new Son();  
        	System.out.println("------------");  
        	Son s2 = new Son("林青霞");  
    	}  
	} 

输出结果：

	Father的无参构造方法  
	Son的无参构造方法  
	------------  
	Father的无参构造方法  
	Son的带参构造方法 


	class Father {  
      
    	/*public Father() { 
        	System.out.println("Father的无参构造方法"); 
    	} 
    	*/  
      
    	public Father(String name) {  
        	System.out.println("Father的带参构造方法");  
    	}  
	}  
  
	class Son extends Father {  
    	public Son() {  
        	super("随便给");  
        	System.out.println("Son的无参构造方法");  
        	//super("随便给");  
    	}  
      
    	public Son(String name) {  
        	//super("随便给");  
        	this();  
        	System.out.println("Son的带参构造方法");  
    	}  
	}  
  
	class ExtendsDemo7 {  
    	public static void main(String[] args) {  
        	Son s = new Son();  
        	System.out.println("----------------");  
        	Son ss = new Son("林青霞");  
    	}  
	}  

输出结果：

	Father的带参构造方法  
	Son的无参构造方法  
	----------------  
	Father的带参构造方法  
	Son的无参构造方法  
	Son的带参构造方法  


**知识点介绍（非常重要）**

**继承(掌握)**

**(1)把多个类中相同的成员给提取出来定义到一个独立的类中。然后让这多个类和该独立的类产生一个关系，**

  这多个类就具备了这些内容。这个关系叫继承。
  
**(2)Java中如何表示继承呢?格式是什么呢?**

A:用关键字extends表示

B:格式：

class 子类名 extends 父类名 {}

**(3)继承的好处：**


A:提高了代码的复用性

B:提高了代码的维护性

C:让类与类产生了一个关系，是多态的前提

**(4)继承的弊端：**

A:让类的耦合性增强。这样某个类的改变，就会影响其他和该类相关的类。

原则：低耦合，高内聚。

耦合：类与类的关系

内聚：自己完成某件事情的能力

B:打破了封装性

**(5)Java中继承的特点**

A:Java中类只支持单继承

B:Java中可以多层(重)继承(继承体系)

**(6)继承的注意事项：**

A:子类不能继承父类的私有成员

B:子类不能继承父类的构造方法，但是可以通过super去访问

C:不要为了部分功能而去继承

**(7)什么时候使用继承呢?**

A:继承体现的是：is a的关系。

B:采用假设法

**(8)Java继承中的成员关系**

A:成员变量

a:子类的成员变量名称和父类中的成员变量名称不一样，直接访问

b:子类的成员变量名称和父类中的成员变量名称一样，这个怎么访问呢?

子类的方法访问变量的查找顺序：就近原则

在子类方法的局部范围找，有就使用。

在子类的成员范围找，有就使用。

在父类的成员范围找，有就使用。

找不到，就报错。

B:构造方法

a:子类的构造方法默认会去访问父类的无参构造方法

1:子类中所有的构造方法默认都会访问父类中空参数的构造方法

2:为什么呢?

因为子类会继承父类中的数据，可能还会使用父类的数据。

所以，子类初始化之前，一定要先完成父类数据的初始化。父类的初始化是调用方法区中的构造方法进行初始化，不会创建父类对象，对象是要new关键字来创建的（

new关键字有两个作用。一是分配内存，

创建对象。二是调用构造方法，完成对象的初始化工作。完成这两步之后，才算创建了一个完整的Java对象。

所以new子类的时候，调用父类的构造方法不是创建了一个父类对象，而是只对它的数据进行初始化，那么父类这些数据存储在哪里呢？通俗说子类对象内存区域中会划一部分区域给父类的数据的存储，即子类对象内存中封装了父类的初始化数据，创建子类对象时，父类的数据就是子类的对象的一部分，不存在独立的父类的对象，所有的东西在一起才是一个完整的子类的对象）注意：子

类每一个构造方法的第一条语句默认都是：super();





注：只供学习交流使用

转载地址：http://blog.csdn.net/u010687392/article/details/42388585