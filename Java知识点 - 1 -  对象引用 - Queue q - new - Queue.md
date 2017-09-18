#Java对象引用、对象赋值、子类对象赋值给父类引用机制

为便于说明，我们先定义一个简单的类：

       class Vehicle {

       int passengers;      

       int fuelcap;

       int mpg;

                   }
                   
有了这个模板，就可以用它来创建对象：

       Vehicle veh1 = new Vehicle();

通常把这条语句的动作称之为创建一个对象，其实，它包含了四个动作。

1）右边的“new Vehicle”，是以Vehicle类为模板，在堆空间里创建一个Vehicle类对象（也简称为Vehicle对象）。

2）末尾的()意味着，在对象创建后，立即调用Vehicle类的构造函数，对刚生成的对象进行初始化。构造函数是肯定有的。如果你没写，Java会给你补上一个默认的构造函数。

3）左边的“Vehicle veh 1”创建了一个Vehicle类引用变量。所谓Vehicle类引用，就是以后可以用来指向Vehicle对象的对象引用。

4）“=”操作符使对象引用指向刚创建的那个Vehicle对象。

先看下面的程序：

	StringBuffer s;

	s = new StringBuffer("Hello World!");

第一个语句仅为引用(reference)分配了空间，而第二个语句则通过调用类(StringBuffer)的构造函数StringBuffer(String str)为类生成了一个实例（或称为对象）。这两个操作被完成后，对象的内容则可通过s进行访问——在Java里都是通过引用来操纵对象的。

 

Java对象和引用的关系可以说是互相关联，却又彼此独立。彼此独立主要表现在：引用是可以改变的，它可以指向别的对象，譬如上面的s，你可以给它另外的对象，如：

	s = new StringBuffer("Java");

这样一来，s就和它指向的第一个对象脱离关系。

从存储空间上来说，对象和引用也是独立的，它们存储在不同的地方，对象一般存储在堆中，而引用存储在速度更快的堆栈中。


引用可以指向不同的对象，对象也可以被多个引用操纵，如：

	StringBuffer s1 = s;