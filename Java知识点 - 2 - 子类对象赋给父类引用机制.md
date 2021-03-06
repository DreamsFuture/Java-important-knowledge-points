#子类对象赋值给父类引用机制和好处

**子类继承父类所有方法，名字没有改变，但是子类把这些方法里面的内容做了差异化修改，父类引用自然可以使用名字来调用子类中继承过来的方法，但是底层内容已经改变，显示出了子类的差异，也体现了引用名的区别**

**好处：把不同的子类对象都当作父类来看，可以屏蔽不同子类对象之间的差异，写出通用的代码，做出通用的编程，以适应需求的不断变化。**

赋值之后，父对象就可以根据当前赋值给它的子对象的特性以不同的方式运作。也就是说，父亲的行为像儿子，而不是儿子的行为像父亲。

举个例子：从一个基类中派生，响应一个虚命令，产生不同的结果。

比如从某个基类继承出多个对象，其基类有一个虚方法Todoit，然后其子类也有这个方法，但行为不同，然后这些子对象中的任何一个可以赋给其基类的对象，这样其基类的对象就可以执行不同的操作了。实际上你是在通过其基类来访问其子对象的，你要做的就是一个赋值操作。

使用继承性的结果就是可以创建一个类的家族，在认识这个类的家族时，就是把导出类的对象当作基类的对象，这种认识又叫作upcasting。这样认识的重要性在于：我们可以只针对基类写出一段程序，但它可以适应于这个类的家族，因为编译器会自动就找出合适的对象来执行操作。这种现象又称为多态性。而实现多态性的手段又叫称**动态绑定(dynamic binding)**。

简单的说，建立一个父类的对象，它的内容可以是这个父类的，也可以是它的子类的,当子类拥有和父类同样的函数，当使用这个对象调用这个函数的时候，定义这个对象的类（也就是父类）里的同名函数将被调用，当在父类里的这个函数前加virtual关键字，那么子类的同名函数将被调用。

在java中：

多态，是面向对象的程序设计语言最核心的特征。多态，意味着一个对象有着多重特征，可以在特定的情况下，表现不同的状态，从而对应着不同的属性和方法。从程序设计的角度而言，多态可以这样来实现（以java语言为例）：

	public interface Parent {
		public void simpleCall();
		}
	public class Child_A implements Parent{
		public void simpleCall(){
			//具体的实现细节；
		}
	}
	public class Child_B implements Parent{
		public void simpleCall(){
			//具体的实现细节；
		}
	}


然后，我们就可以看到多态所展示的特性了：

	Parent pa = new Child_A();
	
pa.simpleCall()则显然是调用Child_A的方法；

	Parent pa = new Child_B();
	
pa.simpleCall()则是在调用Child_B的方法。所以，我们对于抽象的父类或者接口给出了我们的具体实现后，pa 可以完全不用管实现的细节，只访问我们定义的方法，就可以了。事实上，这就是多态所起的作用，可以实现控制反转这在大量的J2EE轻量级框架中被用到，比如Spring的依赖注射机制。

**那这么使用的优点是什么，为什么要这么用？可以用这几个关键词来概括：多态、动态链接，向上转型**

也有人说这是面向接口编程，可以降低程序的耦合性，即调用者不必关心调用的是哪个对象，只需要针对接口编程就可以了，被调用者对于调用者是完全透明的。让你更关注父类能做什么,而不去关心子类是具体怎么做的,你可以随时替换一个子类,也就是随时替换一个具体实现,而不用修改其他.

以后结合设计模式（如工厂模式，代理模式）和反射机制可能有更深理解。

**下面介绍java的多态性和其中的动态链接，向上转型：**

面向对象的三个特征：**封装、继承和多态**；

封装隐藏了类的内部实现机制，可以在不影响使用者的前提下修改类的内部结构，同时保护了数据；

继承是为了重用父类代码，子类继承父类就拥有了父类的成员。

方法的重写、重载与动态连接构成多态性。Java之所以引入多态的概念，原因之一是它在类的继承问题上和C++不同，后者允许多继承，这确实给其带来的非常强大的功能，但是复杂的继承关系也给C++开发者带来了更大的麻烦，为了规避风险，Java只允许单继承，派生类与基类间有IS-A的关系（即“猫”is a “动物”）。这样做虽然保证了继承关系的简单明了，但是势必在功能上有很大的限制，所以，Java引入了多态性的概念以弥补这点的不足，此外，抽象类和接口也是解决单继承规定限制的重要手段。同时，多态也是面向对象编程的精髓所在。 

理解多态，首先要知道“**向上转型**”。

我定义了一个子类Cat，它继承了Animal类，那么后者就是前者是父类。我可以通过 

	Cat c = new Cat(); 
实例化一个Cat的对象，这个不难理解。但当我这样定义时： 

	Animal a = new Cat(); 
这代表什么意思呢？ 

很简单，它表示我定义了一个Animal类型的引用，指向新建的Cat类型的对象。由于Cat是继承自它的父类Animal，所以Animal类型的引用是可以指向Cat类型的对象的。这就是“向上转型”。

**那么这样做有什么意义呢？**

因为子类是对父类的一个改进和扩充，所以一般子类在功能上较父类更强大，属性较父类更独特， 定义一个父类类型的引用指向一个子类的对象既可以使用子类强大的功能，又可以抽取父类的共性。 所以，父类类型的引用可以调用父类中定义的所有属性和方法，而对于子类中定义而父类中没有的方法，父类引用是无法调用的； 

那什么是动态链接呢？当父类中的一个方法只有在父类中定义而在子类中没有重写的情况下，才可以被父类类型的引用调用； 对于父类中定义的方法，如果子类中重写了该方法，那么父类类型的引用将会调用子类中的这个方法，这就是动态连接。 

   对于多态，可以总结以下几点：

**一、使用父类类型的引用指向子类的对象；**

**二、该引用只能调用父类中定义的方法和变量；**

**三、如果子类中重写了父类中的一个方法，那么在调用这个方法的时候，将会调用子类中的这个方法；（动态连接、动态调用）**

**四、变量不能被重写（覆盖），”重写“的概念只针对方法，如果在子类中”重写“了父类中的变量，那么在编译时会报错。**

注：以上内容均为转载，自己整理集合仅供学习交流使用。

链接地址：

<http://blog.csdn.net/gideal_wang/article/details/4913965>

<https://zhidao.baidu.com/question/462751386.html>