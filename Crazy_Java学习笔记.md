#Crazy_Java学习笔记

__要做到边学边记   2017/9/19__

#2.数据类型和运算符

Java语言支持的类型分为两类：基本类型（Primitive Type）和引用类型（Reference Type）
基本类型包括boolean类型和数值类型。数值类型有整数类型和浮点数类型。
整数类型包括byte short int long char
浮点类型包括float double

引用类型包括类、接口和数组类型，还有一种特殊的null类型

这一块感觉各类语言看来都是差不多的，我就直接跳了，开始做习题练手

1.输出九九乘法表
```java
class Test {

    public static void main(String[] args) {
        
        for (int i = 1 ; i < 10 ; i ++ ) {
            for (int j = 1 ; j <= i ; j++ ) {
                System.out.print(j + "*" + i + "=" + i * j + ",");
            }
            System.out.println();
        }

    }
}

```

2.输出等腰三角形
```java
import java.util.Arrays;

class Test {

    public static void main(String[] args) {
        
        printAngle(4);

    }

    public static void printAngle(int n) {


        for(int i = 1 ; i <= n ; i++) { 

            char[] str = new char[2 * n - 1];
            int start = n - i;
            int end = n + i -2;
            Arrays.fill(str , start , end + 1 , '*');
            for(char j : str) {
                System.out.print(j);
            }
            System.out.println();
        }

    }

}


```


得到的结果如下：

            *
           ***
          *****
         *******

#3.数组
##数组的初始化
静态初始化：arrayName = new type[] {elements1 , elements2 , ...}
动态初始化：arrayName = new type[length]
执行动态初始化时只需指定数组长度，即为每个数组元素指定所需内存的空间，并为他们分配初始值

#4.面向对象（上）

##类和对象（非常重要的一章）

Java面向对象的三大特征：封装、继承和多态
三个访问控制修饰符：private、protected、public
提供了extends关键字来让子类继承父类
构造器用于对类实例进行初始化操作，构造器支持重载
修饰符：public、protected、private、static、final、abstract
public、protected、private三个最多只能出现其一
abstract和final最多只能出现其一
static是一个特殊的关键字。static修饰的成员表面它属于这个类本身，而不属于该类的单个实例，通常把static修饰的成员变量和方法也称为类变量、类方法
__注意：静态成员不能直接访问非静态成员__

栈内存里存放有引用变量，引用变量并未真正存储对象的成员变量
堆内存里存放有对象。Java程序不允许直接访问堆内存中的对象，只能通过该对象的引用操作该对象
如果没有任何变量指向堆内存中的对象，Java的垃圾回收机制就会回收某个对象，释放该对象所占的内存区
__方法在栈区__


this关键字总是指向调用该方法的对象。看下面这个例子
```java
public class Dog {

    public void jump() {
        System.out.println("jumping!!!");
    }

    public void run() {
        this.jump();
        System.out.println("running!!!");
    }
}
```

Java也允许对象的一个成员直接调用另外一个成员
这是修改后的Dog

```java
public class Dog {

    public void jump() {
        System.out.println("jumping!!!");
    }

    public void run() {

        //修改处
        jump();
        System.out.println("running!!!");
    }
}
```

对于static修饰的方法而言，则可以使用类来__直接__调用方法（这句话隐藏的意思是可以间接调用，__看下面的例子__），换句话说，就是static修饰的方法属于类，不属于对象！！！如果在static修饰的方法中使用this关键字，则这个关键字就无法指向合适的对象。所以static修饰的方法中不能使用this引用。__因此，Java语法规定静态成员不能直接访问非静态成员__

虽然Java允许使用对象来调用static修饰的成员变量、方法，但是他实际上依然是用类来调用的。
在实际编程中最好不要这么做，这样会降低程序的可读性、明确性

程序可以像访问普通引用变量一样访问this，甚至可以把this当做普通方法的返回值
```java
public class ReturnThis{

    public int age;
    public ReturnThis grow() {
        age++;
        return this;
    }

    public static void main(String[] args) {

        //static看来是可以间接调用非static的
        ReturnThis rt = new ReturnThis();
        rt.grow().grow().grow();
        System.out.println(rt.age);
    }
}

```


##形参个数可变的方法
在定义方法时，在最后一个形参的类型后增加三点（...），则表明该形参可以接受多个参数值，多个参数值__被当做数组__传入。例子如下：
```java
class Test {

    public static void main(String[] args) {
        
        test(10 , "a" , "b" , "c");

    }

    public static void test (int a , String... books) {

        for (String tmp : books) {
            System.out.println(tmp);
        }

        System.out.println(a);
    }

}

```
从这个例子可以看出，形参个数可变的参数本质上就是一个数组参数，也就是说下面两个方法的效果是完全一样的
    
    public static void test(int a , String... books);

    public static void test(int a , String[] books);

如果采用第二种则传入的应该是一个数组：

    test(5 , new String[]{"a" , "b" , "c"});

还要注意的是，数组形式的形参可以在任意位置，而个数可变的形参只能处于形参表的最后，即一个方法中最多只有一个__个数可变的形参__

##方法重载
同一个类中包含了两个或两个以上方法的方法名相同，但形参列表不同，则称为方法重载

```java
class Test {


    public void test() {
        System.out.println("no args");

    }

    public void test(String msg) {
        System.out.println("hava args : " + msg);
    }

    public static void main(String[] args) {
        
        Test t = new Test();
        //调用第一个方法
        t.test();
        //调用第二个方法
        t.test("hello");

    }
}

```

<a href="https://www.zhihu.com/question/35477541">原来的字体consolas不是等宽的，换了个字体Microsoft Yahei Mono</a>


##成员变量和局部变量
成员变量无需显式初始化，只要为一个类定义了类变量或实例变量，系统就会进行默认初始化，其赋值规则与数组动态初始化时数组元素的赋值规则完全相同
局部变量除了形参外，都必须显式初始化，否则不可访问
定义局部变量后，系统并未给这个变量分配内存空间，直至给这个变量赋初始值是，系统才会为局部变量分配内存，并将初始值保存到这块内存中
局部变量不属于任何类或实例，因此他总是保存在其所在方法的栈内存中

栈内存中的变量无须系统垃圾回收，往往随方法或代码块的运行结束而结束

当程序需要访问__类变量__时，尽量使用类作为主调，这样可以避免程序产生歧义，提高可读性

##隐藏和封装
private:   被修饰的成员 只能在当前类的内部被访问
default:   被修饰的成员 可以被相同包下的其他类访问
protected: 被修饰的成员 既可以被同一个包中的其他类访问，
           也可以被不同包中的子类访问。
           使用protected来修饰通常是希望其子类来重写这个方法
public:    被修饰的成员 可以被所有类访问

注意：对于外部类而言，只能有两种访问级别：public和默认（default）；如果一个Java源文件里定义了一个public修饰的类，则这个源文件的文件名必须与public修饰的类的类名相同

package语句必须作为源文件的第一条非注释性语句，一个源文件只能指定一个包

import会导入当前包下的所有类，而其子包里的类则不会被导入

Java默认所有的源文件导入java.lang包下的所有类

在一些极端的情况下，import语句也不行，此时只能在源文件中使用类全名。例如：
    
    import java.util.*;
    import java.sql.*;
    如果在接下来的程序中使用Date类，则会引起错误。因为这两个包里都有Date类
    此时就要指定包下的Date类
    java.sql.Date d = new java.sql.Date();

import static 用于静态导入，可以导入单个静态成员变量、方法，也可以是全部

导入后可以简化程序：

```java
import static java.lang.System.*;
import static java.lang.Math.*;
public class StaticImportTest {

    public static void main(String[] args) {

        //out是java.lang.System类的静态成员变量
        //PI是java.lang.Math类的静态成员变量
        out.println(PI);
        out.println(sqrt(256));
    } 
}
```


##构造器
实际上，当程序员调用构造器时，系统会先为该对象分配内存空间，并为这个对象执行默认初始化。也就是说当系统开始执行构造器的执行体系之前，系统已经创建了一个对象。当构造器的执行体执行结束后，这个对象作为构造器的返回值被返回。
一旦有了自定义的构造器，系统就不再提供默认的构造器
构造器可以重载
构造器通常设置成public

构造器B完全包含构造器A，可以使用this关键字。例子如下：
```java
public class Apple {

    public String name;
    public String color;
    public double weight;

    public Apple() {}

    public Apple(String name , String color) {
        this.name = name;
        this.color = color;
    }

    public Apple(String name , String color , double weight) {

        //通过this调用另一个重载的构造器的初始化代码
        this(name , color);
        this.weight = weight;
    }
}

```

注：使用this调用另一个重载的构造器只能在构造器中使用，而且必须作为构造器执行体的__第一条__语句。
这样可以尽量避免相同代码的出现，充分复用每一段代码，既可以让代码变得简洁，也可以降低软件的维护成本

##类的继承

    Java里子类继承父类的语法格式：
    修饰符 class SubClass extends SuperClass {
        //类定义部分
    }

Java语言摒弃了C++中难以理解的多继承特征，即每个类最多只有一个直接父类
java.lang.Object类是所有类的父类，因此所有的Java对象都可调用java.lang.Object类所定义的实例方法

```java
public class BaseClass {
    public int a = 9;
}

public class SubClass extends BaseClass {
    public int a = 4;
    public void accessOwner() {
        System.out.println(a);
    }

    public void accessBase() {
        System.out.println(super.a);
    }

    public static void main(String[] args) {
        SubClass sc = new SubClass();
        sc.accessOwner();
        sc.accessBase();
    }
}

```

注：一个Java源文件中只能有一个public修饰的类，这个源文件的文件名必须与public修饰的类的类名相同

子类不会获得父类的构造器，但是子类构造器里可以调用父类构造器的初始化代码，类似于一个构造器调用另一个重载的构造器。下面来看个例子。

```java
class BaseClass {
    
    public double size;
    public String name;
    public BaseClass(double size , String name) {
        this.size = size;
        this.name = name;
    }
}

public class SubClass extends BaseClass {
    
    public String color;
    public SubClass(double size , String name , String color) {

        //这和之前的一个构造器调用另外一个构造器的使用方式是差不多的
        super(size , name);
        this.color = color;
    }


    public static void main(String[] args) {

        SubClass sc = new SubClass(1 , "Alan" , "red");
        System.out.println(sc.size + "--" + sc.color + "--" + sc.name);
    }
}
```

注：super调用父类构造器也必须出现在子类构造器执行体的第一行。所以this调用和super调用不会同时出现

下面来看看继承时，各个构造器执行的顺序

```java
class Creature {

    public Creature() {
        System.out.println("Creature no arg constructor");
    }

}

class Animal extends Creature {

    public Animal(String name) {
        System.out.println("Animal one arg constructor , name is " + name);
    }

    public Animal(String name , int age) {
        this(name);
        System.out.println("Animal two args constructor , age is " + age);
    }
}

public class Wolf extends Animal {

    public Wolf() {
        super("Wolf" , 20);
        System.out.println("Wolf no arg constructor");
    }

    public static void main(String[] args) {
        new Wolf();
    }
}


```

    执行后结果如下：

    Creature no arg constructor
    Animal one arg constructor , name is Wolf
    Animal two args constructor , age is 20
    Wolf no arg constructor

从这运行商来看，创建任何对象总是从该类所在继承树的最顶层类的构造器开始执行，然后依次向下执行，最后才执行本类的构造器。如果某个父类通过this调用了同类中重载的构造器，就会依次执行此父类的多个构造器

##多态

Java引用变量有两个类型：编译时类型 和 运行时类型
编译时类型由声明该变量时使用的类型决定
运行时类型由实际赋给该变量的对象决定
如果这两个类型不一致，就可能出现所谓的多态
 

```java
class BaseClass {
    
    public int book = 6;
    public void base() {
        System.out.println("father");
    }

    public void test() {
        System.out.println("father test func");
    }
}

public class SubClass extends BaseClass {
    
    public String book = "SubClass";
    
    //对父类的test()方法进行了重写
    public void test() {
        System.out.println("son test func");
    }

    public void sub() {
        System.out.println("son sub func");
    }

    public static void main(String[] args) {

        BaseClass tmp = new SubClass();

        System.out.println(tmp.book);//输出 6 

        tmp.base();//执行从父类继承到的base()方法

        tmp.test();//执行当前类的test()方法

    }
}

```

    运行结果如下：
    6
    father
    son test func
结果说明：引用变量tmp编译时类型是BaseClass，而在运行时类型是SubClass
所以当调用test()方法时，实际上调用的是BaseClass里的方法，这就是所谓的多态性
与方法不同的是，对象的实例变量不具备多态性，因此输出的是6
动态绑定

知乎上找到的比较好的记忆方法：

__成员变量__
编译看左边(父类),运行看左边(父类)
__成员方法__
编译看左边(父类)，运行看右边(子类)。动态绑定
__静态方法__
编译看左边(父类)，运行看左边(父类)。
(静态和类相关，算不上重写，所以，访问还是左边的)
__只有非静态的成员方法,编译看左边,运行看右边__

__多态的弊端：不能使用子类特有的成员属性和子类特有的成员方法。__因为“BaseClass tmp = new SubClass();” 进行了向上转型，子类的特有的成员属性和成员方法不能用了，所以，若子类重载了父类的方法，则tmp无法调用；而如果子类重写了父类的方法，则tmp可以调用子类重写的方法。

不过在实际开发过程中，最常用的还是接口式多态，比如经常使用的“List<T> list = new ArrayList<>();”这个里面List<T>其实就是一个接口！！！接口里提供了很多方法，ArrayList里面把List接口里的方法很多都重写了，list能直接调用接口里的方法。

在实际开发中，经常会听到“提供了接口，调用接口里的方法类似的话”，接口式多态的好处就是：我们只要使用接口里的方法，无须管他如何实现（如果有另外一个小组负责接口的实现类的实现），程序写好后，使用了接口的方法的地方无须修改，只需要修改接口实现类里的方法的代码即可。
像“List<T> list = new ArrayList<>();”，如果有一天换成了“List<T> list = new ABList<>();”，代码无须改动，方法还是照常使用！！！这有多方便啊！！！多态赛高！！！



把子类对象赋给父类引用变量时，被称为向上转型（upcasting）


instanceof运算符作用：判断前一个对象是否是后一个类的实例

```java
public class Person {

    public static void main(String[] args) {
        
        Object hello = "Hello";
        System.out.println(hello instanceof Object );
        System.out.println(hello instanceof Comparable );

    }
}

```
hello这个变量在编译时时Object类，但是在运行时是String类

##初始化块
```java

public class Test {

    //1
    static Test test = new Test(); 

    //2
    {
        System.out.println("normal");
    }

    //3
    static{
        System.out.println("static");
    }

    //4
    public static void main(String [] args){
        Test test = new Test();
    }

}

```

    输出结果：
    normal
    static
    normal

说明：先执行静态初始化块和静态成员变量，执行顺序与源程序中的排列顺序相同，然后再执行非静态初始化块

#5.面向对象（下）

##包装类（Wrapper Class）

基本数据类型和包装类的对应关系

| 基本数据类型     | 包装类        |
| :-------------: | :-----------: |
| byte             | Byte          |
| short            | Short         |
| int              | Integer       |
| long             | Long          |
| char             | Character     |
| float            | Float         |
| double           | Double        |
| boolean          | Boolean       |


__自动装箱__（Autoboxing）：把一个基本类型变量直接赋给对应的包装类变量，或者赋给Object变量

__自动拆箱__（AutoUnboxing)：允许直接把包装类对象直接赋给一个对应的基本类型变量

```java
public class AutoBoxingUnboxing {

    public static void main(String[] args) {
        Integer inObj = 5;
        Object boolObj = true;
        int it = inObj;

        if (boolObj instanceof Boolean) {

            boolean b = (Boolean)boolObj;
            System.out.println(b);
            
        }
    }
}

```

将基本类型变量转换成字符串

基本数据类型>>>>>------String.valueOf(primitive)---------->>>>>String对象

基本数据类型<<<<<-------WrapperClass.parseXxx()-----------<<<<<String对象

parse：解析的意思


```java
public class Primitive2String {

    public static void main(String[] args) {
        
        String intStr = "123";
        int it1 = Integer.parseInt(intStr);
        int it2 = new Integer(intStr);
        System.out.println(it2);

        String floatStr = "4.56";
        float ft1 = Float.parseFloat(floatStr);
        float ft2 = new Float(floatStr);
        System.out.println(ft2);

        String ftStr = String.valueOf(2.345f);
        System.out.println(ftStr);

        String dbStr = String.valueOf(3.344);
        System.out.println(dbStr);

        String boolStr = String.valueOf(true);
        System.out.println(boolStr.toUpperCase());
        
    }
}

```

以上2017.9.21 22:05

##处理对象

###toString()
```java
class Person {
    private String name;
    public Person (String name) {
        this.name = name;
    }
}

public class PrintObject {
    
    public static void main(String[] args) {
        Person p = new Person("sun");
        System.out.println(p);
        System.out.println(p.toString());
    }
}

```

    运行结果：
    Person@15db9742
    Person@15db9742

toString()方法是Object类里的一个实例方法，所有的Java类都是Object类的子类，因此所有的Java对象都具有toString()方法

toString()是一个__自我描述__方法：当程序员直接打印该对象时，系统会输出该对象的“自我描述”信息，用以告诉外界该对象具有的状态信息

toString()返回的是该对象实现类的 __“类名+@+hashCode”__ 值，这个返回值并不能真正实现“自我描述”的功能，因此如果需要自定义这个功能，就要重写Object类的toString()方法，例子如下：

```java
class Apple {

    private String color;
    private double weight;

    public Apple (String color , double weight) {
        this.color = color;
        this.weight = weight;

    }

    public String toString() {
        return "the apple's color is " + color + 
                ", weight is " + weight + "kg";
    }
}

public class ToStringTest {

    public static void main(String[] args) {
        Apple a = new Apple("red" , 1);
        System.out.println(a);
    }
}

```

    输出：
    the apple's color is red, weight is 1.0kg

###== 和 equals方法

常量池 ？？？？？？？？？？？？？

Java会使用常量池来管理曾经用过的字符串直接量，例如执行String a = "java";语句之后，常量池中就会缓存一个字符串"java";如果程序再次执行String b = "java";系统将会让b直接指向常量池中的"java"字符串，因此a==b将会返回true

JVM常量池保证相同的字符串直接量只有一个，不会产生多个副本

可以重写equals()方法来建立自定义的相等标准，甚至在极端情况下，你可以让Person对象和Dog对象相等

```java
class Person {

    private String name;
    private String idStr;
    public Person(){}
    public Person(String name , String idStr) {
        this.name = name;
        this.idStr = idStr;
    }

    public String getIdStr() {
        return idStr;
    }

    public boolean equals(Object obj) {

        if (this == obj) {
            return true;
        }
        if (obj != null && obj.getClass() == Person.class) {
            Person personObj = (Person)obj;
            if(this.getIdStr().equals(personObj.getIdStr())) {
                return true;
            }
        }
        return false;
        
    }
}

public class OverrideEqualsRight {
    public static void main(String[] args) {
        Person p1 = new Person("A" , "123");
        Person p2 = new Person("B" , "123");
        Person p3 = new Person("C" , "456");
        System.out.println("A equals B ? " + p1.equals(p2));
        System.out.println("A equals C ? " + p1.equals(p3));
    }
}

```

通常，正确重写equals()方法应满足：

- 自反性：对任意的x，x.equals(x)一定返回true
- 对称性：如果y.equals(x)返回true，那x.equals(y)也返回true
- 传递性：（简略）x=y y=z 则 x=z
- 一致性：对任意的x和y，如果对象中用于等价比较的信息没有改变，那么无论调用x.equals(y)多少次返回的结果应该保持一致
- 对任何不适null的x，x.equals(null)一定返回false

###单例（Singleton）类
如果一个类始终只能创建一个实例，则这个类被称为单例类

为了避免其他类自由创建该类的实例，应该吧该类的构造器使用private修饰，从而把该类的所有构造器隐藏起来。

根据良好封装的原则：一旦把该类的构造器隐藏起来，就需要提供一个public方法作为该类的访问点，用于创建该类的对象，且该方法必须使用static修饰（因为调用该方法之前还不存在对象，英雌调用该方法的不可能是对象，只能是类）

除此之外，该类还必须缓存已经创建的对象，否则该类无法知道是否曾经创建过对象，也就无法保证只创建一个对象。

为此该类需要使用一个成员变量来保存曾经创建的对象，因为该成员变量需要被上面的静态方法访问，故成员变量必须使用static修饰。来看下面的例子

```java

class Singleton {

    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {

        if (instance == null) {
            
            instance = new Singleton();
        }

        return instance;
    }
}

public class SingletonTest {
    public static void main(String[] args) {
        
        //创建Singleton对象不能通过构造器
        //只能通过getInstance方法来得到实例
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();
        System.out.println(s1 == s2);
    }
}

```

##final修饰符

final修饰符可用于修饰类、变量和方法，用于表示它修饰的类、变量和方法__不可改变__

final修饰的变量一旦获得了初始值，该final变量的值就不能被重新赋值

###final成员变量

Java语法规定：__final修饰的成员变量必须由程序员显式地指定初始值__

这是因为如果没有为这些final修饰的成员变量指定初始值，那么这些成员变量的值将一直是系统默认分配的0、false、null、'\u0000'，这些成员变量也就完全失去了存在的意义

final修饰的类变量，要么在定义该类变量时指定初始值，要么在静态初始化块中为该类变量指定初始值

注意：与普通成员变量不同的是，final成员变量（包括实例变量和类变量）必须由程序员显式初始haul，系统不会对final成员进行隐式初始化

###final修饰的引用类型变量

对于引用类型变量而言，它保存的仅仅是一个引用，final只保证这个引用类型变量所引用的地址不会改变，即一直引用同一个对象，__但是对象的内容却是可以改变的__

###可执行“宏替换”的final变量
当定义final变量是为该变量指定了初始值，而且该初始值可以在编译时就确定下来，那么这个final变量本质上就是一个“宏变量”。编译器会把程序中所有用到该变量的地方直接替换成该变量的值。

这里看个关于常量池的例子，具体看<a href="http://www.cnblogs.com/dreamroute/p/5946272.html">这个网址</a>

```java
public class StringJoinTest {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "hel" + "lo";

        //true 编译器在编译阶段就可以确定s2的值
        //系统在会让s2直接指向常量池中缓存的"hello"字符串
        System.out.println(s1 == s2); 

        String s3 = "hel";
        String s4 = "lo";
        String s5 = s3 + s4;

        //由于s3和s4只是两个普通变量，编译器不会执行“宏替换”
        //因此编译器无法再编译时确定s3的值
        //等到运行时,拼接成的新字符串，在堆中地址不确定，
        //不可能与方法区常量池中的s1地址相同
        System.out.println(s1 == s5);//false
    }
}

```

###final方法
final修饰的方法不可被重写，如果处于某些原因，不希望子类重写父类的某个方法，则可以使用final修饰该方法。但要注意的是，用final修饰的方法仅仅是不能被重写，并不是不能被重载
```java
public class FinalOverload {

    public final void test() {}
    public final void test(String arg) {}
}


```

###final类
final修饰的类不可以有子类，例如java.lang.Math类就是一个final类，它不可以有子类

##抽象类

在某些具体的情况下，某个父类只是知道其子类应该包含怎样的方法，但无法准确地知道这些子类如何实现这些方法。这个时候使用抽象方法就能很方便的应对这种情况

###抽象方法和抽象类

有抽象方法的类只能被定义成抽象类，抽象类里可以没有抽象方法

抽象方法和抽象类的规则如下：

- 抽象类和抽象方法必须使用abstract修饰符来修饰，抽象方法不能有方法体
- 抽象类不能被实例化
- 抽象类可以包含成员变量、方法（普通/抽象方法都可）、构造器、初始化块、内部类（接口、枚举）。抽象类的构造器不能用于创建实例，主要用于被其子类调用
- 含有抽象方法的类
    + 直接定义了一个抽象方法
    + 继承了一个抽象父类，但没有完全实现父类包含的抽象方法
    + 实现了一个接口，但没有完全实现接口包含的抽象方法

抽象方法：  public abstract void test();

  空方法：  public void test() {}


下面来看一个具体的例子

```java
public abstract class Shape {

    {
        System.out.println("execute Shape's Initialization block");
    }

    private String color;

    public abstract double calPerimeter();

    public abstract String getType();

    public Shape(){}

    public Shape(String color) {

        System.out.println("execute Shape's constructor");
        this.color = color;

    }

    public String getColor() {
        return color;
    }
}

public class Triangle extends Shape {

    //define three edges
    private double a;
    private double b;
    private double c;

    public Triangle(String color , double a , double b , double c) {
        super(color);
        this.setSides(a , b , c);
    }

    public void setSides(double a , double b , double c) {
        if (a >= b + c || b >= a + c || c >= a + b) {
            System.out.println("success");
            return;
        }

        this.a = a;
        this.b = b;
        this.c = c;
    }

    public double calPerimeter() {
        return a + b + c;
    }

    public String getType() {
        return "Triangle";
    }

}

public class Circle extends Shape {

    private double radius;
    public Circle(String color , double radius) {
        super(color);
        this.radius = radius;
    }

    public void setRadius(double radius) {
        this.radius = radius;
    }

    public double calPerimeter() {
        return 2 * Math.PI * radius;
    }

    public String getType() {
        
        return getColor() + " Circle";
    }

    public static void main(String[] args) {
        Shape s1 = new Circle("yellow" , 3);
        System.out.println(s1.getType());
        System.out.println(s1.calPerimeter());
        System.out.println();
        
        Shape s2 = new Triangle("black" , 3 , 4 ,5);
        System.out.println(s2.getType());
        System.out.println(s2.calPerimeter());
    }
}

```

    输出结果：
    execute Shape's Initialization block
    execute Shape's constructor
    yellow Circle
    18.84955592153876

    execute Shape's Initialization block
    execute Shape's constructor
    Triangle
    12.0

利用抽象类和抽象方法的优势，可以更好地发挥多态的作用

final修饰的类不能被继承且不能被重写。因此final和abstract永远不能同时使用

因为abstract修饰的方法没有方法体（“{}”），通过类调用该方法肯定会出现错误。所以static和abstract不能同时修饰__某个方法__，__但是它们可以同时修饰内部类__

abstract关键字修饰的方法必须被其子类重写才有意义，否则这个方法将永远不会有方法体，因此abstract方法不能定义为private访问权限，即private和abstract不能同时修饰方法

abstract不能用于修饰构造器，抽象类里定义的构造器只能是普通构造器

##Java8改进的接口

接口定义了一种规范，定义了某一批类所要遵守的规范，接口不关心这些类的内部状态数据，也不关心这些类里方法的实现细节，它只规定这批类里必须提供某些方法。可见，接口是从多个相似类中抽象出来的规范，接口不提供任何实现。接口体现的是规范和实现分离的设计哲学

接口定义的基本语法

    [修饰符] interface 接口名 extends 父接口1，父接口2...
    {
        零个到多个常量定义...
        零个到多个抽象方法定义...
        零个到多个内部类、接口、枚举定义...
        零个到多个默认方法或类方法定义...
    }

- 修饰符可以是public或省略，如果省略，则只有在相同包结构下才可以访问该接口
- 接口只能继承接口，不能继承类，但是类能继承和实现接口

接口里可以包含的成员变量（只能是静态常量）、方法（只能是抽象实例方法、类方法或默认方法）、内部类（包括内部接口、枚举）

接口定义的是多个类共同的公共行为规范，因此接口类的所有成员都是public访问权限。定义接口成员时，可以省略访问控制修饰符，如果指定访问控制修饰符，则只能使用public访问控制修饰符。

对于接口里定义的静态常量而言，系统会自动为这些成员变量增加static和final两个修饰符，也就是说，不管是否使用public static final修饰符，接口里的成员变量总是使用这三个修饰符来修饰。因此即使另一个类处于不同包下，也可以通过接口来访问接口里的成员变量。因为接口里没有构造器和初始化块，因此接口里定义的成员变量只能在定义时指定默认值。

接口里定义成员变量采用如下两行代码的结果完全一样

    int MAX_SIZE = 50;
    public static final int MAX_SIZE = 50;

接口里定义的方法只能是抽象/类/默认方法，如果不是定义默认（default）方法，系统将自动为普通方法增加abstract修饰符；定义接口里的普通方法时不管是否使用public abstract修饰符，接口里的普通方法总是使用public abstract来修饰。接口里的普通方法不能有方法体；但类方法、默认方法都必须由方法体

默认方法必须使用default修饰，该方法不能使用default修饰，无论程序是否指定，默认方法总是使用public修饰。由于默认方法没有static修饰，因此不能直接使用接口来调用默认方法，需要使用接口的实现类的实例来调用这些默认方法

类方法必须使用static，不能使用default，无论程序是否指定，类方法总是使用public修饰。类方法可以直接使用接口来调用



下面定义一个接口

```java
public interface Output {

    int MAX_SIZE = 50;

    void out();

    void getData(String msg);

    //接口里定义默认方法需要使用default修饰
    default void print(String... msgs) {

        for (String msg : msgs) {
            System.out.println(msg);
        }
    }

    default void test() {

        System.out.println("default test()");
    }

    //在接口中定义类方法，需要使用static修饰
    static String staticTest() {

        return "接口里的类方法";
    }
}

```

从某个角度看，接口可被当成一个特殊的类，因此一个java源文件里最多只能有一个public接口，该源文件的主文件名必须与该接口名相同

###接口的继承

接口继承和类不同。接口支持多继承

```java
interface interfaceA {

    int PROP_A =5;
    void testA();
}

interface interfaceB {

    int PROP_B = 6;
    void testB();
}

interface interfaceC extends interfaceA , intetfaceB {

    int PROP_C = 7;
    void testC();
}

public class InterfaceExtendsTest {
    public static void main(String[] args) {

        System.out.println(interfaceC.PROP_A);
        System.out.println(interfaceC.PROP_B);
        System.out.println(interfaceC.PROP_C);
    }
}

```

###使用接口

接口的主要用途：

- 定义变量，也可用于进行强制类型转换
- 调用接口中定义的常量
- 被其他类实现

一个类可以实现一个或多个接口，继承使用extends（扩展），实现则使用implements（实现）关键字

因为类可以实现多个接口，这也是Java为单继承灵活性不足所做的补充

类实现接口的语法格式如下：

    [修饰符] class 类名 extends 父类 implements 接口1，接口2... {
        类体部分
    }

实现接口与继承父类相似，一样可以获得所实现接口里定义的常量（成员变量）、方法（包括抽象方法和默认方法）

一个类implements一个或多个接口后，这个类必须完全实现这些接口里所定义的全部抽象方法（重写）；否则，该类将保留从父接口那里继承到的抽象方法，该类也必须定义成抽象类。

可以把实现接口理解成一种特殊的继承，相当于类继承了一个__彻底抽象的类__（相当于除了默认方法外，所有方法都是抽象方法的类）

下面看一个实现接口的类

```java
interface Product {
    
    //抽象方法
    int getProduceTime();
}

interface Output {

    int MAX_CACHE_LINE = 50;

    //抽象方法
    void out();
    
    //抽象方法
    void getData(String msg);

    //接口里定义默认方法需要使用default修饰
    default void print(String... msgs) {

        for (String msg : msgs) {
            System.out.println(msg);
        }
    }

    default void test() {

        System.out.println("default test()");
    }

    //在接口中定义类方法，需要使用static修饰
    static String staticTest() {

        return "接口里的类方法";
    }
}

public class Printer implements Output , Product {
    
    private String[] printData = new String[MAX_CACHE_LINE];
    private int dataNum = 0;
    
    public void out() {
        
        while (dataNum > 0) {
            System.out.println("打印机打印：" + printData[0]);
            //把作业队列整体前移一位，并将剩下的作业数减一
            System.arraycopy(printData, 1, printData, 0, --dataNum);
        }
    }
    
    public void getData(String msg) {
        if (dataNum >= MAX_CACHE_LINE) {
            System.out.println("输出队列已满，添加失败");
        } else {
            printData[dataNum++] = msg;
        }
    }
    
    public int getProduceTime() {
        return 45;
    }
    
    public static void main(String[] args) {
        Output o = new Printer();
        o.getData("hello");
        o.getData("world");
        o.out();
        o.print("fuck" , "the" , "world");
        o.test();
        
        Product p = new Printer();
        System.out.println(p.getProduceTime());
    }

    Object obj = p;//向上转型
}
```

仿佛Printer类既是Output类的子类，也是Product类的子类，这就是Java提供的模拟多继承

###接口和抽象类

两者的区别：


##内部类

将定义在其他类内部的类就被称为内部类，包含内部类的类也被称为外部类

内部类的作用：

- 内部类隐藏在外部类之内，不允许用一个包中的其他类访问该类。有一个比较形象的例子：Cow类需要组合一个CowLeg对象，CowLeg类只有在Cow类里才有效，离开Cow类之后没有任何意义。在这种情况下就可把CowLeg定义成Cow的内部类。
- 内部类成员可以直接访问外部类的private数据，因为内部类被当成其外部类成员，同一个类的成员之间可以互相访问。但外部类不能方位内部类的实现细节，比如内部类的成员变量
- 匿名内部类适合用于创建那些仅需要一次使用的类
- 外部类有两种访问权限：包访问权限（省略访问控制符）和公开访问权限（public）
- 内部类比外部类可以多实用三个修饰符：private、protected、static
- 非静态内部类不能拥有静态成员

下面程序在Cow类里定义了一个CowLeg非静态内部类，并在CowLeg类的实例方法中直接访问Cow的private访问权限的实例变量

```java
public class Cow {
    
    private double weight;
    public Cow() {}
    public Cow(double weight) {
        this.weight = weight;
    }
    
    //定义一个非静态内部类
    private class CowLeg {
        
        private double length;
        private String color;
        
        public CowLeg() {}
        public CowLeg(double length , String color) {
            this.length = length;
            this.color = color;
            
        }
        
        public void info() {
            
            System.out.println("CowLeg's color is " + color + " ,length is " + length + "m");
            
            System.out.println("The Cow's weight is " + weight + "kg");
        }
    }
    
    public void test() {
        CowLeg c1 = new CowLeg(1.12 , "white and black");
        c1.info();
    }
    
    public static void main(String[] args) {
        Cow cow = new Cow(1000);
        cow.test();
    }
}

```
可以发现：在外部类里使用非静态内部类时，与平时使用普通类并没有太大区别

- 静态成员不能访问非静态成员
- 不允许在非静态内部类里定义静态成员

```java
public class StaticTest {
    
    private class In{}

    public static void main(String[] args) {

        //下面的代码将产生异常
        new In();
    }
}
```
说明：类In是类StaticTest的一个外部类成员，在main里是无法直接访问的。你会看到错误提示：“静态成员无法访问非静态成员”

###静态内部类

外部类的上一级程序单元是包，所以不能使用static修饰，而内部类的上一级程序单元是外部类，使用static修饰可以将内部类变成外部类相关，而不是外部类实例相关。因此static关键字不可修饰外部类，但可修饰内部类

__静态内部类可以包含静态成员，也可以包含非静态成员__，根据静态成员不能访问非静态成员的规则，静态内部类不能访问外部类的实例成员，只能访问外部类的类成员。即使是静态内部类的实例方法也不能访问外部类的实例成员（看下面例子）
__非静态内部类不能拥有静态成员__

以上2017.9.21 22:19

```java
public class StaticInnerClassTest {
    private int prop1 = 5;
    private static int prop2 = 9;
    static class StaticInnerClass {
        //静态内部类里可以包含静态成员
        private static int age;
        public void accessOuterProp() {

            //error,静态内部类无法访问外部类的实例变量
            System.out.println(prop1);

            //下面代码正常
            System.out.println(prop2);
        }
    }
}
```

说明：静态内部类对象是寄生在外部类的类本身中。当静态内部类对象存在时，并不存在一个被它寄生的外部类对象。我的理解是：没有外，哪来的内？

外部类不能__直接__访问静态内部类的成员，但可以使用静态内部类的类名作为调用者来访问静态内部类的类成员，也可以使用静态内部类对象作为调用者来访问静态内部类的实例成员

如果外部类需要访问非静态内部类的成员，则必须显式创建非静态内部类对象来调用访问其实例成员。

```java
public class AccessStaticInnerClass {
    static class StaticInnerClass {
        private static int prop1 = 6;
        private int prop2 = 9;
    }

    public void accessInnerProp() {

        System.out.println(StaticInnerClass.prop1);
        System.out.println(new StaticInnerClass().prop2);
    }
}
```

除此之外，Java还允许在接口里定义内部类，默认使用public static修饰，即只能是静态内部类

###使用内部类

 1. 在外部类内部使用内部类
 2. 在外部类以外使用非静态内部类
    - 省略访问控制符，只能被与外部类处于同一个包中的其他类访问
    - 使用protected，同上+课被外部类的子类访问
    - public，任何地方

在外部类以外的地方__定义内部类__（包括静态和非静态）__变量__，语法格式：

    OuterClass.InnerClass varName

若有包名，还得增加包名前缀

在外部类以外的地方创建非静态内部类实例的语法如下：

    OuterInstance.new InnerConstructor()

下面来看个例子

```java
class Out {

    class In {
        public In(String msg) {
            System.out.println(msg);
        }
    }
}

public class CreateInnerInstance {
    public static void main(String[] args) {

        Out.In in = new Out().new In("test msg");

        /*
        等价于以下三行
        Out.In in;
        Out out = new Out();
        in = out.new In("test msg");
        */
    }
}
```

非静态内部类的构造器必须使用外部类对象来调用

3. 在外部类以外使用静态内部类

因为静态内部类是外部类类相关的，因此创建静态内部类对象时无须创建外部类对象
```java
class StaticOut {

    static class StaticIn {
        public StaticIn() {
            System.out.println("静态内部类的构造器");
        }
    }
}

public class CreateStaticInnerInstance {
    public static void main(String[] args) {
        StaticOut StaticIn in = new StaticOut.StaticIn();
    }
}
```

###局部内部类

如果把一个内部类放在方法里定义，则这个内部类就是一个局部内部类，仅在该方法里有效。由于局部内部类不能再外部类的方法以外的地方使用，因此局部内部类也不能使用访问控制符和static修饰符。

其实，不管是局部变量还是局部内部类，他们的上一级程序单元都是方法，而不是类，使用static修饰它们是没有任何意义的

在实际开发中很少使用局部内部类，太鸡肋了

###匿名内部类
匿名内部类适合创建那种只需要一次使用的类。创建匿名内部类时会立即创建一个该类的实例，这个类定义立即消失，匿名内部类不能重复使用

定义匿名内部类的格式如下：

    new 实现接口() | 父类构造器(实参列表){
        //匿名内部类的类体部分
    }

从上面的定义可以看出，匿名内部类必须继承一个父类，或实现一个接口，但最多只能继承一个父类，或实现一个接口。除此之外，还有如下两条规则：
- 匿名内部类不能使抽象类
- 匿名内部类不能定义构造器

继承接口
```java
package f921;

interface Product {
    public double getPrice();
    public String getName();
}

public class AnonymousTest {
    public void test(Product p) {
        System.out.println("购买了一个" + p.getName()
        + ",花掉了" + p.getPrice());
    }
    
    public static void main(String[] args) {
        AnonymousTest ta = new AnonymousTest();

        //此处传入其匿名实现类的实例
        ta.test(new Product() {
            public double getPrice() {
                return 567.8;
            }
            public String getName() {
                return "AGP显卡";
            }
        });
    }
}

```


继承父类
```java
abstract class Device {
    private String name;
    public abstract double getPrice();
    public Device() {}
    public Device(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
}

public class AnonymousInner {
    
    public void test(Device d) {
        System.out.println("buy a " + d.getName() + 
                ", pay " + d.getPrice());
    }
    
    public static void main(String[] args) {
        AnonymousInner ai = new AnonymousInner();
        ai.test(new Device("电子示波器") {
            public double getPrice() {
                return 67.8;
            }
        });
        
        Device d = new Device() {
            {
                System.out.println("匿名内部类的初始化块");
            }
            public double getPrice() {
                return 56.2;
            }
            public String getName() {
                return "键盘";
            }
        };
        
        ai.test(d);
        
    }
}

```

匿名抽象类。。。不是很明白为啥？？？？？？？？？？？？？？
为啥抽象类可以创建实例了？？？？

匿名内部类可以为抽象类甚至普通类创建实例

##Lamabda表达式

不是很想看？？？？？？？？？

##枚举类

用enum定义枚举类，他是特殊的类，但普通类有的东西他都有，但枚举类终究不是普通类，有以下几个区别：

- 枚举类可以实现一个或多个接口，使用enum定义的枚举类默认继承了java.lang.Enum类，而不是默认继承Object类，因此枚举类不能显式继承其他父类
- 使用enum定义、非抽象的枚举类默认会使用__final__修饰，因此枚举类不能派生子类
- 枚举类的构造器__只能使用__private访问控制符，如果省略或强制指定，__都只能是private__
- 枚举类的所有实例必须在枚举类的第一行显式列出，否则这个枚举类永远都不能产生实例，列出这些实例时，系统会自动添加public static finall修饰，无须显式添加

枚举类默认提供了一个values()方法，可以很方便地遍历所有的枚举值

```java
public enum SeasonEnum {
    SPRING , SUMMER , FALL , WINTER;
}

public class EnumTest {
    
    public void judge(SeasonEnum s) {
        switch (s) {
        case SPRING:
            System.out.println("春");
            break;
        
        case SUMMER:
            System.out.println("夏");
            break;
        case FALL:
            System.out.println("秋");
            break;
        case WINTER:
            System.out.println("冬");
            break;
        }
    }
    
    public static void main(String[] args) {
        for(SeasonEnum s : SeasonEnum.values()) {
            System.out.println(s);
        }
        
        new EnumTest().judge(SeasonEnum.SPRING);
    }
    
}
```


枚举类通常设计成不可变类，即其成员变量值不允许改变，这样会更安全，因此建议将枚举类的成员变量都使用private final修饰

一旦为枚举类显式定义了带参数的构造器，列出枚举值时就必须对应地传入参数

通过让每个枚举值分别来实现抽象方法，是的不同的枚举值调用该方法时具有不同的行为方式
```java
package f922;

public enum Gender {

    MALE("男") 
    {
        public void info() {
            System.out.println("这个枚举值代表男性");
        }
    },
    FEMALE("女") 
    {
        public void info() {
            System.out.println("这个枚举值代表女性");
        }
    };
    
    private final String name;
    
    private Gender(String name) {
        this.name = name;
    }
    
    public String getName() {
        return this.name;
    }
    
    public abstract void info();
    
    public static void main(String[] args) {
        System.out.println(Gender.MALE.getName());
        System.out.println(Gender.FEMALE.getName());
        Gender.MALE.info();
        Gender.FEMALE.info();
    }
    
}

```
以上2017.9.22 22:20


#6.Java基础类库

Java提供了System类和Runtime类来与程序的运行平台进行交互

###System类
不能创建System类的对象，可以调用类变量和类方法

System还提供了一个identityHashCode(Object x)方法，该方法返回指定对象的精确的hashCode值，也就是根据该对象的地址计算得到的hashCode值。

```java
public class IdentityHashCodeTest {
    public static void main(String[] args) {
        String s1 = new String("hello");
        String s2 = new String("hello");
        //String重写了hashCode(),根据字符序列来计算hashCode值
        System.out.println(s1.hashCode() + "----" + s2.hashCode());

        //identityHashCode()是根据对象地址计算hashCode值的
        System.out.println(System.identityHashCode(s1)
                + "----" + System.identityHashCode(s2));
        String s3 = "hello";
        String s4 = "hello";
        System.out.println(System.identityHashCode(s3)
                + "----" + System.identityHashCode(s4));            
    }
}
```

###Runtime类

简单看看即可

```java
public class RuntimeTest {
    public static void main(String[] args) {
        Runtime rt = Runtime.getRuntime();
        System.out.println("处理器数量：" + rt.availableProcessors());
        System.out.println("空闲内存数：" + rt.freeMemory());
        System.out.println("总内存数：" + rt.totalMemory());
        System.out.println("可用最大内存数：" + rt.maxMemory());
    }
}
```

    运行结果：
    处理器数量：4
    空闲内存数：126929960
    总内存数：128974848
    可用最大内存数：1888485376

Runtime类可以直接启动一个进程来运行操作系统命令
```java
public class ExecTest {
    public static void main(String[] args) 
        throws Exception {
            Runtime rt = Runtime.getRuntime();
            rt.exec("notepad.exe");
        }
    
}
```

##常用类

Object类提供的clone()方法使用了protected修饰，因此该方法只能被子类重写或调用
```java
class Address {
    String detail;
    public Address(String detail) {
        this.detail = detail;
    }
}

class User implements Cloneable{
    int age;
    Address address;
    public User(int age) {
        this.age = age;
        address = new Address("Zhejiang");
    }
    public User clone() throws CloneNotSupportedException
    {
        return (User)super.clone();
    }
}

public class CloneTest {
    public static void main(String[] args) 
        throws CloneNotSupportedException
    {
        User u1 = new User(19);
        User u2 = u1.clone(); //得到一个u1的副本
        System.out.println(u1 == u2);//false
        System.out.println(u1.address == u2.address);//true
    }
}

```
克隆出来的对象只是原有对象的副本，如果实例变量的类型是引用类型，Object的Clone机制也只是简单地复制这个引用变量，他们依然指向堆内存中的同一个实例，这只是一种浅克隆

###String、StringBuffer和StringBuilder类

###ThreadLocalRandom与Random
ThreakLocalRandom是Random的增强版。在并发环境下，使用TLR来代替R可以减少多线程资源竞争，最终保证系统具有更好地线程安全性

ThreakLocalRandom提供了一个静态的current()方法来获取ThreakLocalRandom对象

```java
import java.util.Arrays;
import java.util.Random;

public class RandomTest {
    public static void main(String[] args) {
        Random rand = new Random();
        System.out.println("rand.nextBoolean(): " + rand.nextBoolean());
        byte[] buffer = new byte[16];
        
        //这里之间传入数组对象
        rand.nextBytes(buffer);
        System.out.println(Arrays.toString(buffer));
        
        //生成0.0~1.0之间的伪随机double数
        System.out.println("rand.nextDouble(): " + rand.nextDouble());
    }
}
```

    输出结果：
    rand.nextBoolean(): false
    [-58, -22, -113, -5, -24, -66, -47, 118, -42, 100, -106, -105, 45, -122, -80, -111]
    rand.nextDouble(): 0.8024352892614993

当两个Random对象种子相同时，它们会产生相同的数字序列。当使用默认的种子构造Random对象是，它们属于同一个种子

```java
import java.util.Random;

public class SeedTest {
    public static void main(String[] args) {
        Random r1 = new Random(50);
        System.out.println("第一个种子为50的Random对象");
        System.out.println("r1.nextBoolean():\t" + r1.nextBoolean());
        System.out.println("r1.nextInt():\t\t" + r1.nextInt());
        System.out.println("r1.nextDouble():\t" + r1.nextDouble());
        System.out.println("r1.nextGaussian():\t" + r1.nextGaussian());
        System.out.println("----------------------");
        Random r2 = new Random(50);
        System.out.println("第二个种子为50的Random对象");
        System.out.println("r2.nextBoolean():\t" + r2.nextBoolean());
        System.out.println("r2.nextInt():\t\t" + r2.nextInt());
        System.out.println("r2.nextDouble():\t" + r2.nextDouble());
        System.out.println("r2.nextGaussian():\t" + r2.nextGaussian());
        System.out.println("----------------------");
        Random r3 = new Random(20);
        System.out.println("第三个种子为20的Random对象");
        System.out.println("r3.nextBoolean():\t" + r3.nextBoolean());
        System.out.println("r3.nextInt():\t\t" + r3.nextInt());
        System.out.println("r3.nextDouble():\t" + r3.nextDouble());
        System.out.println("r3.nextGaussian():\t" + r3.nextGaussian());
        System.out.println("----------------------");
    }
}

```

    输出结果：
    第一个种子为50的Random对象
    r1.nextBoolean():   true
    r1.nextInt():       -1727040520
    r1.nextDouble():    0.6141579720626675
    r1.nextGaussian():  2.377650302287946
    ----------------------
    第二个种子为50的Random对象
    r2.nextBoolean():   true
    r2.nextInt():       -1727040520
    r2.nextDouble():    0.6141579720626675
    r2.nextGaussian():  2.377650302287946
    ----------------------
    第三个种子为20的Random对象
    r3.nextBoolean():   true
    r3.nextInt():       -1704868423
    r3.nextDouble():    0.20600366582289698
    r3.nextGaussian():  0.43976686256647524
    ----------------------

可以看出来，这是一种伪随机

为了避免两个Random对象产生相同的数字序列，通常推荐使用当前时间作为Random对象的种子，如下：

    Random rand = new Random(System.currentTimeMillis());

ThreakLocalRandom的使用方式和Random基本一样，只是在ThreakLocalRandom是用current()获取ThreakLocalRandom对象

    ThreakLocalRandom rand = ThreakLocalRandom.current();
    int val = rand.nextInt(4 , 20);
    int val2 = rand.nextDouble(2.0 , 10.0);

###BigDecimal类

创建BigDecimal对象时，不要直接使用double浮点数作为构造器参数来调用BigDecimal构造器，否则供养会发生精度丢失的问题。

通常建议优先使用基于String的构造器 BigDecimal(String val)

如果必须使用double浮点数作为BigDecimal构造器的参数时，应通过BigDecimal.valueOf(double value)静态方法来创建BigDecimal对象
    
        BigDecimal f1 = new BigDecimal("0.1");
        BigDecimal f2 = BigDecimal.valueOf(0.01);
        BigDecimal f2 = BigDecimal.valueOf(0.01);//这个会造成精度损失

##Java8的日期、时间类

Date类历史悠久，很多已经过时了，Java8就提供了一套全新的日期时间库

- Date(): 生成一个代表当前日期时间的Date对象
- Date(long date): 根据指定的long型整数来生成一个Date()对象，该参数表示与GMT 1970年1月1日00:00:00之间的时间差，以毫秒作为计时单位
- long getTime()：返回该时间对应的long型整数，即与GMT 1970年1月1日00:00:00之间的时间差，以毫秒作为计时单位
- void setTime(long time): 设置该Date对象的时间

```java
import java.util.Date;

public class DateTest {
    public static void main(String[] args) {
        Date d1 = new Date();
        Date d2 = new Date(System.currentTimeMillis() + 10000000);
        System.out.println(d1);
        System.out.println(d2);
        System.out.println(d1.compareTo(d2));
        System.out.println(d1.before(d2));
    }
}
```

    输出结果：
    Sat Sep 23 13:09:38 CST 2017
    Sat Sep 23 15:56:18 CST 2017
    -1
    true

总体来说，Date是一个设计相当糟糕的类。__官方推荐使用Calendar工具类__

###Calendar类
因为Date类在设计上存在一些缺陷，所以Java提供了Calendar类来更好地处理日期和时间。Calendar本身是一个抽象类，不能直接实例化，但它提供了几个静态的getInstance()方法来获取Calendar对象。Java本身提供了一个GregorianCalendar类（即公历）

##Java8新增的日期、时间格式器

这一块就直接省略了，我感觉不是很重要，需要用到的时候再看好了


#7.Java集合
2017.9.26

集合类也被称为容器类。

Java的集合类主要由两个接口派生而出：Collection和Map，也称Java集合框架的根接口
对于Set、List、Queue、Map四种集合，最常用的实现类是HashSet、TreeSet、ArrayList、ArrayDeque、LinkedList、HashMap、TreeMap


2017.10.2
休息了两天，接下来要全力以赴了！！！
##Collection和Iterator

__使用Lambda表达式遍历集合？？？？__

Iterator接口也是Java集合框架的成员，他是Collection接口的父接口，主要作用是遍历Collection集合中的元素，Iterator对象也被称为__迭代器__

```java
import java.util.*;
import static java.lang.System.*;

public class CollectionTest {
    public static void main(String[] args) {
        Collection c = new HashSet();
        c.add("Jack");
        c.add("Fiboor");
        c.add("hello");
        out.println(c);
        
        //使用迭代器
        Iterator it = c.iterator();
        while (it.hasNext()) {
            //it.next()方法返回的数据类型是Object类型
            //因此需要强制类型转换
            String name = (String)it.next();
            out.println(name);
            if (name.equals("hello")) {
                
                //从集合中删除上一次next()方法返回的元素
                it.remove();
                
                //当使用Iterator迭代访问Collection集合元素时
                //Collection集合里的元素不能被改变
                //c.remove(name);
            }
            
            name = "goodbye";
        }
        out.println(c);     
        c.add("fu");
        out.println(c);
        c.removeIf(ele -> ((String)ele).length() < 3);
        out.println(c);     
    }
}
```

__注意__：在迭代、使用foreach循环中访问集合元素时，该集合不能被改变

Java8为Collection集合新增了一个removeIf(Predicate filter)方法，该方法将批量删除符合filter条件的所有元素。该方法需要一个Predicate（谓词）对象，Predicate也是函数式接口，因此可使用Lambda表达式作为参数

```java
import java.util.*;
import java.util.function.Predicate;

import static java.lang.System.*;

public class PredicateTest2 {
    public static void main(String[] args) {
        Collection c = new HashSet();
        c.add("Jack");
        c.add("Fiboor");
        c.add("hello");
        
        //统计字符串长度大于4的数量
        out.println(calAll(c , ele -> ((String)ele).length() > 4));
        
    }
    
    public static int calAll(Collection c , Predicate p) {
        int total = 0;
        for (Object obj : c) {
            //使用Predicate的test()方法判断该对象是否满足Predicate指定的条件
            if(p.test(obj)) {
                total++;
            }
        }
        return total;
    }
}

```

Java8还新增了Stream、IntStream、LongStream、DoubleStream等流式API。Stream是一个通用的流接口，而IntStream、LongStream、DoubleStream则代表类型为int、long、double的流。这些流式还有对应的Builder。例如Stream.Builder、、、以此类推，开发者可以通过这些Builder来创建对应的流

独立使用Stream的步骤如下：

1. 使用Stream或XxxStream的builder()类方法创建该Stream对应的Builder.
2. 重复调用Bulider的add()方法向该流中添加多个元素
3. 调用Builder的build()方法获取对应的Stream
4. 调用Stream的聚集方法

```java
import java.util.stream.*;

public class IntStreamTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        IntStream is = IntStream.builder()
                .add(2)
                .add(13)
                .add(-21)
                .build();
        
        //下面这些代码只能执行一次
//      System.out.println("is所有元素的最大值：" + is.max().getAsInt());
//      System.out.println("is所有元素的最小值：" + is.min().getAsInt());
//      System.out.println("is所有元素的总和：" + is.sum());
//      System.out.println("is所有元素的总数：" + is.count());
//      System.out.println("is所有元素的平均值：" + is.average());
//      System.out.println("is所有元素的平方是否都大于20："
//              + is.allMatch(ele -> ele * ele > 20));
//      System.out.println("is是否包含元素的平方大于20："
//              + is.anyMatch(ele -> ele * ele > 20));
        
        //将is映射成一个新的Stream，新Stream的每个元素时原Stream元素的2倍+1
        IntStream newIs = is.map(ele -> ele * 2 + 1);
        
        //遍历
        newIs.forEach(System.out::println);
    }

}

```

可以使用流式API来操作集合，Collection接口提供了一个stream()默认方法，该方法可以返回该集合对应的流。由于Stream可以对集合进行整体的聚集操作，因此将上面的calAll()的程序改写

```java
import java.util.Collection;
import java.util.HashSet;

public class CollectionStream {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Collection c = new HashSet();
        c.add("Jack");
        c.add("Fiboor");
        c.add("hello");
        
        //返回数量
        System.out.println("字符串长度大于4的有几个？ " + 
        c.stream().filter(ele -> ((String)ele).length() > 4).count());
        
        c.stream().mapToInt(ele -> ((String)ele).length()).forEach(System.out::println);
    }

}

```

这里的mapToInt()方法将Stream对象转化成IntStream对象，这是个中间方法，所以可以继续调用forEach()方法遍历流中的元素

##Set集合

无序；不允许包含相同的元素；方法与Collection基本相同

###HashSet类

大多时候使用Set集合时就是使用这个实现类。HashSet按Hash算法来存储集合中的元素，因此具有很好的存取和查找性能。特点如下：
 
 - 不能保证元素的排列顺序。
 - HashSet不是同步的，如果多个线程同时访问一个HashSet，必须通过代码来保证其同步
 - 集合元素值可以使null

```java
package f102;

import java.util.HashSet;

class A {
    public boolean equals(Object obj) {
        return true;
    }
}

class B {
    public int hashCode() {
        return 1;
    }
}

class C {
    public int hashCode() {
        return 2;
    }
    
    public boolean equals(Object obj) {
        return true;
    }
}


public class HashSetTest {
    public static void main(String[] args) {
        HashSet books = new HashSet();
        books.add(new A());
        books.add(new A());
        books.add(new B());
        books.add(new B());
        books.add(new C());
        books.add(new C());
        System.out.println(books);
    }
}

```
输出结果：

    [f102.B@1, f102.B@1, f102.C@2, f102.A@15db9742, f102.A@6d06d69c]

从上述结果可以得到以下结论：HashSet集合判断两个元素相等的标准是两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法返回值也相等

如果两个对象通过equals()方法返回true，hashCode()方法返回值不同，这会导致HashSet会把这两个对象保存在HashSet的不同位置，从而使两个对象都可以添加成功，这就与Set集合的规则想冲突了

如果两个对象的hashCode()方法返回的hashCode值相同，但equals()返回false时，HashSet将把他们保存在同一个位置，这个位置会用链式结构来保存多个对象，这将会导致其性能下降

需要注意的是：当把一个对象放入HashSet中时，如果需要重写该对象对应类的equals()方法，则也应该重写其hashCode()方法，规则是：如果两个对象通过equals()方法比较返回true，这两个对象的hashCode()值也应该相同，这个规则__尽量做到__，这样能避免很多麻烦。


__hashCode()对于HashSet的重要性：__
hash算法的价值在于速度，当需要查询集合中某个元素时，hash算法可以直接根据该元素的hashCode值计算出该元素的存储位置，从而快速定位该元素。类比数组，数组是所有能存储一组元素里最快的数据结构，数组可以包含多个元素，每个元素都有索引，如果需要访问某个数组元素，只需提供该元素的索引，接下来即根据该索引计算该元素在内存里的存储位置。HashSet则是用hashCode值来计算存储位置。HashSet与数组相比其优势在于可以自由增加HashSet长度。因此，当从HashSet中访问元素时，HashSet先计算该元素的hashCode值，然后直接到该hashCode值对应的位置去取出该元素————这就是HashSet速度很快的原因

下面给出重写hashCode()方法的基本规则

- 在程序运行过程中，同一个对象多次调用hashCode()方法应该返回相同的值
- 如果两个对象通过equals()方法比较返回true，这两个对象的hashCode()值也应该相同
- 对象中用作equals()方法比较标准的实例变量，都应该用于计算hashCode()值

下面给出重写hashCode()方法的一般步骤

1. 把对象内每个有意义的实例变量（即每个参与equals()方法比较标准的实例变量）计算出一个int类型的hashCode值。计算方式如下表

|            实例变量类型            |               计算方式              |
|------------------------------------|-------------------------------------|
| boolean                            | hashCode = (f ? 0 : 1)              |
| 整数类型（byte、short、char、int） | hashCode = (int)f                   |
| long                               | hashCode = (int)f;                  |
| float                              | hashCode = Float.floatToIntBits(f)  |
| double                             | long l = Double.doubleToLongBits(f) |
|                                    | hashCode = (int)(1^(1>>>32))        |
| long                               | hashCode = (int)(f^(f>>>32))        |
| 引用类型                           | hashCode = f.hashCode()             |




2. 用第一步计算出来的多个hashCode值组合计算出一个hashCode值返回(这里为了避免直接相加产生偶然相等，可以为各实例变量的hashCode值乘以任意一个质数后再相加)

代码如下：

    return f1.hashCode() * 19  + (int)f2 * 31;


如果向HashSet中添加一个可变对象后，后面程序修改了该可变对象的实例变量，则可能导致它与集合中的其他元素相同（equals()返回true，hashCode也相等），这就有可能导致HashSet中包含两个相同的对象

```java
import java.util.HashSet;
import java.util.Iterator;

class R {
    int count;
    
    public R (int count) {
        this.count = count;
    }
    
    public String toString() {
        return "R[count:" + count + "]";
    }
    
    public boolean equals(Object obj) {
        if (this == obj) {
            //这个比的是引用对象的地址
            System.out.println("flag1");
            return true;
        }
        
        if (obj != null && obj.getClass() == R.class) {
            R r = (R)obj;
            System.out.println("flag2");
            return this.count == r.count;
        }
        System.out.println("flag3");
        return false;
    }
    
    public int hashCode() {
        return this.count;
    }
}

public class HashSetTest2 {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        HashSet hs = new HashSet();
        hs.add(new R(5));
        hs.add(new R(-3));
        hs.add(new R(9));
        hs.add(new R(-2));
        
        //打印HashSet集合
        System.out.println(hs);
        
        //取出第一个元素
         Iterator it = hs.iterator();
         R first = (R)it.next();
         //为第一个元素的count实例变量赋值
         first.count = -3;
         //再次输出HashSet，里面有重复元素
         System.out.println(hs);
         //删除count为-3的R对象
         hs.remove(new R(-3));
         //可以看到被删除了一个R元素
         System.out.println(hs);
         
         R first2 = new R(-2);
         System.out.println(hs.contains(new R(-3)));
         System.out.println(hs.contains(new R(-2)));
//       System.out.println(first.hashCode());
//       System.out.println(first2.hashCode());
//       System.out.println(first.equals(first2));      
    }
}

```

HashSet集合里的第1个元素和第2个元素完全相同，这时两个元素已经重复。此时HashSet会比较混乱————————疑问？？？？？？？


###LinkedHashSet类
HashSet还有一个子类LinkedHashSet，在HashSet的基础上，使用链表维护元素的次序，即LinkedHashSet将会按元素的添加顺序来访问集合里的元素

```java
import java.util.LinkedHashSet;

public class LinkedHashSetTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        LinkedHashSet books = new LinkedHashSet();
        books.add("Jack");
        books.add("Alan");
        books.add("Sara");
        books.add("Tom");
        System.out.println(books);
        books.remove("Jack");
        books.add("Jack");  
        System.out.println(books);
    }
}

```

输出结果如下：

    [Jack, Alan, Sara, Tom]
    [Alan, Sara, Tom, Jack]

可以看出是按照顺序添加的

###TreeSet类
TreeSet是SortedSet接口的实现类，正如SortedSet名字所暗示的那样，TreeSet可以确保集合元素处于排序状态

__如果希望TreeSet能正常运作，TreeSet只能添加同一种类型的对象__

对于TreeSet集合而言，它判断两个对象是否相等的__唯一标准__是：两个对象通过compareTo(Object obj)方法比较是否返回0————如果是，TreeSet则会认为他们相等，否则不相等

```java
import java.util.TreeSet;

class Z implements Comparable {
    int age;
    public Z(int age) {
        this.age = age;
    }
    
    public boolean equals(Object obj) {
        return true;
    }
    
    public int compareTo(Object obj) {
        return 1;
    }
}

public class TreeSetTest2 {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        TreeSet set = new TreeSet();
        Z z1 = new Z(6);
        set.add(z1);
        //第二次添加同一个对象输出true，表明添加成功
        System.out.println(set.add(z1));
        //下面输出set集合，将看到有两个元素
        System.out.println(set);
        //修改set集合第一个元素的age变量
        ((Z)(set.first())).age = 9;
        System.out.println( ((Z)(set.last())).age );
    }
}

```

输出结果

    9

实际上TreeSet对象保存的两个元素，实际上是同一个元素。所以当修改了第一个时，第二个也被修改了。

由此需要遵守一个规则：如果两个对象通过equals()方法比较返回true时，这两个对象通过compareTo(Object obj)方法比较应返回0

如果两个对象通过compareTo(Object obj)方法比较返回0时，但它们通过equals()方法比较返回false将很麻烦，因为两个对象通过compareTo(Object obj)方法比较相等，TreeSet不会让第二个元素添加进去，这就与Set集合的规则产生冲突

如果向TreeSet中添加一个可变对象后，并且后面程序修改了该可变对象的实例变量，这将导致它与其他对象的大小顺序发生变化，但TreeSet不会再次调整它们的顺序，甚至可能导致TreeSet中保存的这两个对象通过compareTo(Object obj)方法比较返回0。请看下面程序

```java
import java.util.TreeSet;

class R2 implements Comparable {
    int count;
    public R2(int count) {
        this.count = count;
    }
    
    public String toString() {
        return "R2[count:" + count + "]";
    }
    
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj != null && obj.getClass() == R.class) {
            R r = (R)obj;
            return r.count == this.count;
        }
        return false;
    }
    
    public int compareTo(Object obj) {
        R2 r = (R2)obj;
        return count > r.count ? 1 :
            count < r.count ? -1 : 0;
    }
}

public class TreeSetTest3 {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        TreeSet ts = new TreeSet();
        ts.add(new R2(5));
        ts.add(new R2(-3));
        ts.add(new R2(9));
        ts.add(new R2(-2));
        //可以看出是有序的
        System.out.println(ts);
        
        R2 first = (R2)ts.first();
        first.count = 20;
        //无序
        System.out.println(ts);
        R2 last = (R2)ts.last();
        last.count = -2;
        //无序且重复
        System.out.println(ts);
        //删除被改变的元素，失败
        System.out.println(ts.remove(new R2(-2)));
        System.out.println(ts.remove(new R2(20)));
        //删除没有被改变的元素，成功
        System.out.println(ts.remove(new R2(5)));
        System.out.println(ts);
        //删除被改变的元素，成功，不过要先把重复的给删完
        System.out.println(ts.remove(new R2(-2)));
        System.out.println(ts.remove(new R2(-2)));
        System.out.println(ts.remove(new R2(20)));      
        //System.out.println(ts.remove(new R2(20)));
        System.out.println(ts);

    }
}
```

result:

    [R2[count:-3], R2[count:-2], R2[count:5], R2[count:9]]
    [R2[count:20], R2[count:-2], R2[count:5], R2[count:9]]
    [R2[count:20], R2[count:-2], R2[count:5], R2[count:-2]]
    false
    false
    true
    [R2[count:20], R2[count:-2], R2[count:-2]]
    true
    true
    true
    []

__为了让程序更加健壮，推荐不要修改放入HashSet和TreeSet集合中元素的关键实例变量__


如果需要实现定制排序，例如以降序排列，则可以通过Comparator接口的帮助。该接口里包含一个int compare(T o1 , T o2)方法，该方法用于比较o1和o2的大小。

返回正整数：o1 > o2
返回0 ： o1 == o2
返回负整数： o1 < o2

使用lambda表达式实现定制排序

```java
import java.util.TreeSet;

class M {
    int age;
    public M(int age) {
        this.age = age;
    }
    public String toString() {
        return "M[age:" + age + "]";
    }
}

public class TreeSetTest4 {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        TreeSet ts = new TreeSet((o1 , o2) ->
        {
            M m1 = (M)o1;
            M m2 = (M)o2;
            //逆序
            return m1.age > m2.age ? -1
                    :m1.age < m2.age ? 1 : 0;
        });
        
        ts.add(new M(5));
        ts.add(new M(-3));
        ts.add(new M(9));
        System.out.println(ts);     
    }
}

```

当把M对象添加到ts集合中时，无须M类实现Comparable接口，因为此时TreeSet无须通过M对象本身来比较大小，而是由与TreeSet关联的Lambda表达式来负责集合元素的排序


##List集合
List集合代表一个元素有序、可重复的集合，可通过索引来访问指定位置的集合元素。List集合默认按元素的添加顺序设置元素的索引，从0开始

下面是List集合的常规用法

```java
import java.util.ArrayList;
import java.util.List;

public class ListTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List books = new ArrayList();
        books.add(new String("Jack"));
        books.add(new String("Alan"));
        books.add(new String("Tom"));
        System.out.println(books);
        //将新字符串对象插入在第二个位置
        books.add(1, new String("Sara"));
        System.out.println(books);
        
        for (int i = 0 ; i < books.size() ; i++) {
            System.out.println(books.get(i));
        }
        
        //删除第三个元素
        books.remove(2);
        System.out.println(books);
        
        //判断指定元素在List集合中的位置
        System.out.println(books.indexOf(new String("Tom"))); //__注意点1__
        
        //将第二个元素替换成新的字符串
        books.set(2, new String("TianTian"));
        System.out.println(books);
        //截取子串，最后没包括
        System.out.println(books.subList(0, 2));    
    }
}

```

__注意点1：__这里是通过new关键字创建的新字符串对象，两个字符串显然不是同一个对象，但List的indexOf方法依然可以返回1，这说明List判断两个对象相等只要通过equals()方法比较返回true即可。请看下面程序

```java
import java.util.ArrayList;
import java.util.List;

class AB {
    public boolean equals(Object obj) {
        return true;
    }
}

public class ListTest2 {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List books = new ArrayList();
        books.add(new String("Jack"));
        books.add(new String("Alan"));
        books.add(new String("Tom"));
        System.out.println(books);
        
        //删除
        books.remove(new AB());
        System.out.println(books);
        
        //删除
        books.remove(new AB());
        System.out.println(books);      
    }
}

```

结果如下：

    [Jack, Alan, Tom]
    [Alan, Tom]
    [Tom]

程序试图删除一个A对象，List将会调回该A对象的equals()方法__依次__与集合元素进行比较，若返回true，List将会删除该元素，所以每次都会删除第一个元素

接下来再看看sort()和replaceAll()两个常用方法。sort()方法需要一个Comparator对象控制元素排序，replaceAll()方法则需要一个UnaryOperator来替换所有集合元素，这两个都可以用Lambda表达式作为参数。

```java
import java.util.ArrayList;
import java.util.List;

public class ListTest3 {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List books = new ArrayList();
        books.add(new String("ABCD"));
        books.add(new String("ABC"));
        books.add(new String("AB"));
        books.add(new String("A"));     
        System.out.println(books);
        books.sort((o1 , o2) -> ((String)o1).length() - ((String)o2).length());
        System.out.println(books);
        books.replaceAll(ele->((String)ele).length());
        System.out.println(books);  
    }

}
```

结果如下：

    [ABCD, ABC, AB, A]
    [A, AB, ABC, ABCD]
    [1, 2, 3, 4]

与Set只提供了一个iterator方法不同，List还__额外__提供了一个listIterator()方法，该方法返回一个ListIterator对象，ListIterator接口继承了Iterator接口，在Iterator接口的基础上还增加了如下方法

- boolean hasPrevious()：返回该迭代器关联的集合是否还有上一个元素
- Object previous()：返回该迭代器的上一个元素
- void add(Object obj)：在List集合中的指定位置插入了一个元素

下面程序示范了ListIterator的用法

```java
import java.util.*;

public class ListIteratorTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        String[] books = {"Jack" , "Tom"};
        List bookList = new ArrayList();
        for (int i = 0; i < books.length; i++) {
            bookList.add(books[i]);
        }
        ListIterator lit = bookList.listIterator();
        while (lit.hasNext()) {
            System.out.println(lit.next());
            //通过add向List集合中添加元素
            lit.add("-------分隔符-------");
        }
        
        System.out.println("=====下面开始反向迭代=====");
        while(lit.hasPrevious()) {
            System.out.println(lit.previous());
        }       
    }
}
```

结果如下：

    Jack
    Tom
    =====下面开始反向迭代=====
    -------分隔符-------
    Tom
    -------分隔符-------
    Jack

要说明的是，使用ListIterator迭代List集合时，开始也需要采用__正向迭代__，即先使用next()方法进行迭代


####ArrayList和Vector实现类
ArrayList和Vector作为List的实现类，完全支持List接口的全部功能。这两个都是基于数组实现的List类，当向他们添加元素超出了该数组的长度时，他们的initialCapacity会自动增加。如果要添加大量元素，可使用ensureCapacity(int minCapacity)方法一次性地增加initialCapacity，__这可以减少分配次数，提高性能__

Vector很古老了，从JDK1.0就有了，__尽量少用Vector及其子类__

####固定长度的List
操作数组的工具类：Arrays，该工具类里提供了asList(Object...a)方法，该方法可以把一个数组或指定个数的对象转换成一个List集合，这个List集合时Arrays的内部类ArrayList的实例

```java
import java.util.*;

public class FixedSizeList {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List fixedList = Arrays.asList("Jack" , "Tom");
        System.out.println(fixedList.getClass());
        fixedList.forEach(System.out::println);     
    }
}
```

结果如下：

    class java.util.Arrays$ArrayList
    Jack
    Tom

##Queue集合
Queue用于模拟队列，队列通常是“先进先出”（FIFO）的容器。通常，队列不允许随机访问队列中的元素

###PriorityQueue实现类
PriorityQueue保存队列的顺序是按照队列元素的大小进行重新排序。看下面的程序

```java
import java.util.*;

public class PriorityQueueTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        PriorityQueue pq = new PriorityQueue();
        pq.offer(6);
        pq.offer(-3);
        pq.offer(20);
        pq.offer(18);
        System.out.println(pq);
        System.out.println(pq.poll());
    }

}
```

结果如下：

    [-3, 6, 20, 18]
    -3

因为受到toString()方法的影响，__可能__看到队列里的元素并没有很好地按大小进行排序。实际上，程序多次调用PriorityQueue集合对象的poll()方法，即可看到元素按从小到大的顺序“移出队列”。PriorityQueue不允许插入null元素，它还需要对队列元素进行排列。排序方式：

- 自然排序
- 定制排序

这个跟TreeSet类似，可翻看前面。PriorityQueue队列对元素的要求与TreeSet对元素的要求基本一致。

###Deque接口与ArrayDeque实现类
Deque接口是Queue接口的子接口，它代表一个双端队列，可以当队列用，也可以当栈用。Deque接口提供了一个典型的实现类：ArrayDeque，这是一个基于数组实现的双端队列。ArrayList和ArrayDeque两个集合类的实现机制基本相似。

下面程序示范了把ArrayDeque当成“栈”来使用

```java
import java.util.*;

public class ArrayDequeStack {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        ArrayDeque stack = new ArrayDeque();
        stack.push("A");
        stack.push("B");
        stack.push("C");
        System.out.println(stack);
        //访问第一个元素，但不pop
        System.out.println(stack.peek());
        System.out.println(stack);
        //pop
        System.out.println(stack.pop());
        System.out.println(stack);
    }

}
```

结果如下：

    [C, B, A]
    C
    [C, B, A]
    C
    [B, A]

因此当程序中需要使用“栈”这种数据结构时，推荐使用__ArrayDeque__

ArrayDeque也可以当成队列使用

```java
import java.util.*;

public class ArrayDequeQueue {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        ArrayDeque queue = new ArrayDeque();
        queue.offer("A");
        queue.offer("B");
        queue.offer("C");
        queue.offer("D");
        System.out.println(queue);
        System.out.println(queue.peek());
        System.out.println(queue);
        System.out.println(queue.poll());
        System.out.println(queue);
    }
}
```

结果如下：

    [A, B, C, D]
    A
    [A, B, C, D]
    A
    [B, C, D]

###LinkedList实现类
LinkedList类是List接口的实现类————这意味着他是List集合，可以根据索引来随机访问集合中的元素，除此之外，__LinkedList还实现了Deque接口__，可以被当成双端队列来使用，因此既可以被当成“栈”来使用，也可以当成“队列”使用

```java
import java.util.*;

public class LinkedListTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        LinkedList books = new LinkedList();
        //加入到队列尾部
        books.offer("A");
        //加入到栈的顶部
        books.push("B");
        System.out.println(books);
        //加入到队列顶部
        books.offerFirst("C");
        System.out.println(books);
        //以List的方式（按索引访问的方式）来遍历集合元素
        for (int i = 0; i < books.size(); i++) {
            System.out.println("遍历中：" + books.get(i));
        }
        //访问队列第一个元素
        System.out.println(books.peekFirst());
        //访问队列最后一个元素
        System.out.println(books.peekLast());
        //弹出栈顶元素
        System.out.println(books.pop());
        System.out.println(books);
        //输出并删除最后一个元素
        System.out.println(books.pollLast());
        System.out.println(books);  
    }
}
```

结果如下：

    [B, A]
    [C, B, A]
    遍历中：C
    遍历中：B
    遍历中：A
    C
    A
    C
    [B, A]
    A
    [B]

上述代码展示了LinkedList作为List集合、双端队列、栈的用法，是一个功能非常强大的类。LinkedList与ArrayList、ArrayDeque的实现机制完全不同。ArrayList、ArrayDeque内部以数组的形式来保存集合中的元素，因此随机访问集合元素时有较好的性能；而LinkedList内部以__链表__的形式来保存集合中的元素，因此随机访问集合元素时性能较差，但在插入、删除元素时性能比较出色。

对于一个成熟的Java程序员，在一些性能非常敏感的地方，可能需要慎重选择哪个List实现。总体来说，ArrayList的性能比LinkedList的性能要好，因此大部分时候都应该考虑使用ArrayList。

关于List集合有如下建议：

- 如果需要遍历List集合元素，对于ArrayList，应该使用随机访问方法（get）来遍历集合元素，这样性能更好；对于LinkedList集合，则应使用迭代器（Iterator）来遍历集合元素
- 如果需要经常执行插入、删除操作来改变包含大量数据的List集合大小，可考虑使用LinkedList集合。使用ArrayList可能需要经常重新分配内部数组大小，效果可能较差
- 如果有多个线程需要同时访问List集合中的元素，开发者可以考虑使用Collection将集合包装成线程安全的集合

##Map集合

```java
import java.util.*;

public class MapTest {
    public static void main(String[] args) {
        Map map = new HashMap();
        map.put("Jack", 15);
        map.put("Alan", 20);
        map.put("Sara", 30);
        System.out.println(map);
        System.out.println(map.put("Jack", 100));//返回原先被覆盖的值
        System.out.println("是否包含Jack？ " + map.containsKey("Jack"));
        System.out.println("是否包含20的value？ " + map.containsValue(20));
        for (Object key : map.keySet() ) {
            System.out.println(key + "--->" + map.get(key));
        }
        //移除
        map.remove("Jack");
        System.out.println(map);
    }
}
```

输出结果：

    {Alan=20, Sara=30, Jack=15}
    15
    是否包含Jack？ true
    是否包含20的value？ true
    Alan--->20
    Sara--->30
    Jack--->100
    {Alan=20, Sara=30}

HashMap和Hashtable都是Map接口的典型实现类，但Hashtable是一个古老的Map实现类，现在很少使用。除此之外

- Hashtable是一个线程安全的Map实现，但HashMap是线程不安全的实现，所以HashMap比Hashtable性能要好，但遇到多个线程访问同一个Map对象时，使用Hashtable会更好点
- Hashtable不允许使用null作为key和value；但HashMap可以

请看下面程序

```java
import java.util.HashMap;

public class NullInHashMap {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        HashMap hm = new HashMap();
        hm.put(null, null);
        hm.put("a", null);
        System.out.println(hm);
        hm.put(null, null);//无法再放入
    }

}
```

需要注意的是：尽量少用Hashtable实现类，如果需要创建线程安全的Map实现类，可以通过Collections工具类把HashMap变成线程安全的

与HashSet不能保证元素的顺序一样，HashMap、Hashtable也不能保证顺序。判断两个key相等的标准：两个key通过equals()方法比较返回true，两个key的hashCode值也相等；判断两个value相等的标准更简单：知晓两个对象通过equals()方法比较返回true即可。请看下面程序

```java
import java.util.Hashtable;

class A {
    int count;
    public A (int count) {
        this.count = count;
    }
    public boolean equals(Object obj) {
        if (obj == this) {
            return true;
        }
        if (obj != null && obj.getClass() == A.class) {
            A a = (A)obj;
            return this.count == a.count;
        }
        return false;
    }
    
    public int hashCode() {
        return this.count;
    }
}

class B {
    public boolean equals(Object obj) {
        return true;
    }
}

public class HashtableTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Hashtable ht = new Hashtable();
        ht.put(new A(60000), "Jack");
        ht.put(new A(87563), "Tom");
        ht.put(new A(1232), new B());
        System.out.println(ht);
        
        //由于Hashtable的value中有一个B对象
        //它与任何对象通过equals()方法比较都相等，所以输出true
        System.out.println(ht.contains("sdfsadfsdafasd"));
        //只要两个A对象的count相等，它们通过equals()方法比较返回true，且hashCode值相等
        //Hashtable则会认为他们是相同的key，所以下面输出true
        System.out.println(ht.containsKey(new A(1232)));
        ht.remove(new A(1232));
        System.out.println(ht);
    }
}
```

输出结果：

    {f103.A@ea60=Jack, f103.A@1560b=Tom, f103.A@4d0=f103.B@15db9742}
    true
    true
    {f103.A@ea60=Jack, f103.A@1560b=Tom}

HashMap、Hashtable保存key的方式与HashSet保存集合的方式完全相同，所以对key的要求也是相同的。与HashSet类似的是，如果使用可变对象作为HashMap、Hashtable的key，并且修改了key，则有可能出现程序再也无法准确访问到Map中被修改过的key了。请看下面程序

```java
import java.util.*;

public class HashMapErrorTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        HashMap hm = new HashMap();
        hm.put(new A(60000), "Jack");//这里用的A和上面的一样
        hm.put(new A(87563), "Tom");
        System.out.println(hm);
        //获取keySet集合对应的Iterator迭代器
        Iterator it = hm.keySet().iterator();
        A first = (A)it.next();
        first.count = 87563;
        System.out.println(hm);
        
        //只能删除没有被修改过的key所对应的key-value对
        hm.remove(new A(87563));
        System.out.println(hm);
        
        //无法获取剩下的value，下面都输出null
        System.out.println(hm.get(new A(87563)));
        System.out.println(hm.get(new A(60000)));       
    }
}
```

输出结果：

    {f103.A@ea60=Jack, f103.A@1560b=Tom}
    {f103.A@1560b=Jack, f103.A@1560b=Tom}
    {f103.A@1560b=Jack}
    null
    null

###LinkedHashMap实现类
LinkedHashMap是HashMap的子类，使用双向链表来维护key-value对的次序，该链表负责维护Map的迭代顺序，迭代顺序与key-value对的插入顺序保持一致。其性能略低于HashMap，但在迭代访问时有较好的性能

```java
import java.util.*;

public class LinkedHashMapTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        LinkedHashMap scores = new LinkedHashMap();
        scores.put("Jack", 100);
        scores.put("Tom", 80);
        scores.put("Sara", 90);
        //System.out.println(scores);
        scores.forEach((key , value) -> 
        System.out.println(key + "-->" + value));
    }

}
```

结果：

    Jack-->100
    Tom-->80
    Sara-->90

###使用Properties读写属性文件
Properties类是Hashtable类的子类，其在处理属性文件（Windows里的.ini）特别方便。Properties类可以把Map对象中的key-value对写入属性文件中，也可以把属性文件中的“属性名=属性值”加载到Map对象中。由于属性文件里的属性名、属性值只能是字符串类型，所以Properties里的key、value都是字符串类型

```java
import java.io.*;
import java.util.*;

public class PropertiesTest {

    public static void main(String[] args)
        throws Exception
    {
        Properties props = new Properties();
        //添加属性
        props.setProperty("username", "Alan");
        props.setProperty("password", "123456");
        //保存到a.ini中
        props.store(new FileOutputStream("a.ini"), "comment line");
        
        Properties props2 = new Properties();
        //添加属性
        props2.setProperty("gender", "male");
        //将a.ini文件中的key-value对追加到props2中
        props2.load(new FileInputStream("a.ini") );
        System.out.println(props2);
    }

}
```

    {password=123456, gender=male, username=Alan}

Properties可以把key-value对以XML文件的形式保存起来，也可以从XML文件中加载key-value对，用法与此类似，可自行查阅

###SortedMap接口和TreeMap实现类

Set接口派生出SortedSet子接口，SortedSet接口有一个TreeSet实现类

类比

Map接口派生出SortedMap子接口，SortedMap接口有一个TreeMap实现类。根据key对节点进行排序

- 自然排序
- 定制排序

TreeMap中判断两个key相等的标准是：两个key通过compareTo()方法返回0，TreeMap即认为这两个key是相等的。

如果使用自定义的类作为TreeMap的key，且想让TreeMap良好地工作，则重写该类的equals()方法和compareTo()方法时应保持一致的返回结果：两个key通过equals()方法比较返回true时，他们通过compareTo()方法比较应该会返回0。

TreeMap的基本用法看文档吧，和TreeSet其实差不多

```java
import java.util.*;

class R implements Comparable {
    int count;
    public R(int count) {
        this.count = count;
    }
    
    public String toString() {
        return "R[count:" + count + "]";
    }
    
    public boolean equals(Object obj) {
        if (obj == this) {
            return true;
        }
        if (obj != null && obj.getClass() == R.class) {
            R r = (R)obj;
            return this.count == r.count;
        }
        return false;
    }
    
    public int compareTo(Object obj) {
        R r = (R)obj;
        return count > r.count ? 1 :
            count < r.count ? -1 : 0;
    }
}


public class TreeMapTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        TreeMap tm = new TreeMap();
        tm.put(new R(3), "A");
        tm.put(new R(41), "C");
        tm.put(new R(5), "B");
        System.out.println(tm);
        
        System.out.println(tm.firstEntry());
        System.out.println(tm.lastKey());
        System.out.println(tm.higherKey(new R(5)));
        System.out.println(tm.lowerKey(new R(41)));
        System.out.println(tm.lowerEntry(new R(2)));
        System.out.println(tm.subMap(new R(-1), new R(10)));
    }

}
```

就不解释了，蛮好懂的，不明白的方法查文档

    {R[count:3]=A, R[count:5]=B, R[count:41]=C}
    R[count:3]=A
    R[count:41]
    R[count:41]
    R[count:5]
    null
    {R[count:3]=A, R[count:5]=B

对于一般的应用场景，程序应该多考虑使用HashMap，因为HashMap正是为快速查询设计的，其底层也是用数组来存储key-value对。这个和HashSet类似

##HashSet和HashMap的性能选项



##操作集合的工具类：Collections
Collections能操作Set、List、Map等集合，该工具类提供了大量方法对集合元素进行排序、查询和修改操作，还提供了将集合对象设置为不可变、对集合对象实现同步控制等方法

Collections类中提供了多个synchronizedXxx()方法，该方法可以将指定集合包装成线程同步的集合，从而解决多线程并发访问集合时的线程安全问题

```java
import java.util.*;

public class SynchronizedTest {
    
    Collection c = Collections.synchronizedCollection(new ArrayList());
    List list = Collections.synchronizedList(new ArrayList());
    Set s = Collections.synchronizedSet(new HashSet());
    Map m = Collections.synchronizedMap(new HashMap());
    
}

```

这样就可以直接获取List、Set和Map的线程安全实现版本

###设置不可变集合

```java
import java.util.*;

public class UnmodifiableTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        //创建一个空的、不可改变的List对象
        List unmodifiableList = Collections.emptyList();
        //创建一个只有一个元素，且不可改变的Set对象
        Set unmodifiableSet = Collections.singleton("Jack");
        //创建一个普通的Map对象
        Map scores = new HashMap();
        scores.put("Alan", 100);
        scores.put("Jack", 50);
        scores.put("LaLa", 10);
        //返回普通Map对象对应的不可变版本
        Map unmodifiableMap = Collections.unmodifiableMap(scores);
        //下面会引发错误！！！Error
        unmodifiableList.add("sdfds");
        unmodifiableSet.add("sdfsdfsd");
        unmodifiableMap.put("Peak", 8);
        
    }

}
```

在计算机行业有一条规则：加入任何规则都必须慎之又慎，因为以后无法删除规则

#8.泛型

##泛型入门

小例子
```java
import java.util.*;

public class GenericList {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List<String> strList = new ArrayList<String>();
        strList.add("A");
        strList.add("B");
        //strList.add(5);//会报错，不能加入整型
        strList.forEach(str -> System.out.println(str.length()));
    }

}
```

###泛型的“菱形”语法

简写

    List<String> strList = new ArrayList<>();
    Map<String , Integer> scores = new HashMap<>();

从Java7开始，Java允许构造器后不需要带完整的泛型信息，只要给出一对尖括号（<>）即可，Java可以推断尖括号里应该是什么泛型信息。看下面这个例子

```java
import java.util.*;

public class DiamondTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        List<String> books = new ArrayList<>();
        books.add("Hello World");
        books.add("Fuck the World");
        books.forEach(ele -> System.out.println(ele.length()));
        Map<String , List<String>> schoolsInfo = new HashMap<>();
        List<String> schools = new ArrayList<>();
        schools.add("斜月三星洞");
        schools.add("大闹天宫");
        schoolsInfo.put("孙悟空", schools);
        schoolsInfo.forEach((key , value) ->
        System.out.println(key + "--->" + value));
    }
}

```

使用“菱形”语法可以更好地简化泛型编程

##深入泛型
所谓泛型，就是允许在定义类、接口、方法时使用类型形参，这个类型形参将在声明变量、创建对象、调用方法时动态地指定（即传入实际的类型参数，也可称为类型实参）。

###定义泛型接口、类
请看下面的__代码片段__

```java
public interface List<E> {
    //在该接口，E可作为类型使用
    void add(E x);
    Iterator<E> iterator();
    ...
}

public interface Map<K , V> {
    Set<K> keySet();
    V put(K key, V value);
}
```

几乎所有可以使用普通类型的地方都可以使用这种类型形参。在实际使用时，只要为E传入不同的类型实参，就可以产生好多个不同的接口。比如，__Set<K>是一种特殊的数据类型，是一种与Set不同的数据类型——————可以认为是Set类型的子类__

__注意：__包含泛型声明的类型可以在定义变量、创建对象时传入一个类型实参，从而可以动态生成无数多个逻辑上的子类，但这种子类在物理上并不存在。

```java
public class Apple<T> {
    
    private T info;
    public Apple(){}
    
    public Apple(T info) {
        this.info = info;
    }
    
    public void setInfo(T info) {
        this.info = info;
    }
    
    public T getInfo() {
        return this.info;
    }

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Apple<String> a1 = new Apple<>("苹果");
        Apple<Double> a2 = new Apple<>(5.67);
        System.out.println(a1.getInfo());
    }

}
```

###从泛型类派生子类
当创建了带泛型声明的接口、父类之后，可以为该接口创建实现类，或从该父类派生子类，注意，当使用这些接口、父类时不能再包含类型形参，使用类、接口、方法时应该为类型参数传入实际的类型。比如，想从Apple类派生一个子类，可以这样

    //使用Apple类时为T形参传入String参数
    public class A extends Apple<String>


    //使用Apple类时，没有为T形参传入实际的类型参数也是正确的
    public class A extends Apple
就像上一章，没有使用泛型时，会发出警告，但没报错

###并不存在泛型类
比如可以把ArrayList<String>类当成ArrayList的子类，但实际上，系统并没有为ArrayList<String>生成新的class文件，而且也不会把ArrayList<String>当成新类来处理

    //分别创建List<String>对象和List<Integer>对象
    List<String> l1 = new ArrayList<>();
    List<Integer> l2 = new ArrayList<>();
    //调用getClass()方法来比较l1和l2类是否相等
    System.out.println(l1.getClass() == l2.getClass());//这里会输出“true”

也就是说，不管为泛型的类型形参传入哪一种类型实参，对于Java来说，他们依然被当成同一个类来处理，在内存中也只占用一块内存空间。__所以在静态方法、静态初始化块或静态变量的声明和初始化中不允许使用类型形参__

由于系统中不会真正生成泛型类，__所以instanceof运算符后不能使用泛型类__

##类型通配符

Java泛型的设计原则是：只要代码在编译时没有出现警告，就不会晕倒运行时ClassCastException

###使用类型通配符

假设现在需要定义一个方法，该方法里有一个集合形参，集合形参里的元素类型是不确定的，那该怎样定义呢？看下面代码

    public void test(List<Object> c) {
        for (int i = 0; i < c.size(); i++) {
            System.out.println(c.get(i));
        }
    }

表面上看上面方法的声明是没有问题的，但调用该方法传入的实际参数值可能并不是我们所期望的，例如

    List<String> strList = new ArrayList<>();
    test(strList);

编译上面的程序将发生编译错误。这表明List<String>对象是不能当做List<Object>对象使用，即__List<String>类并不是List<Object>的子类__。同理，可推出Collection<String>不是Collection<Object>的子类型等等。

比如，为了表示各种泛型List的父类，可以使用类型通配符“？”，它的元素类型可以匹配任何类型

    public void test(List<?> c) {
        for (int i = 0; i < c.size(); i++) {
            System.out.println(c.get(i));
        }
    }

现在不管使用任何类型的List来调用它，程序依然可以访问集合c中的元素，其类型是Object，这永远是安全的，因为不管List的真实类型是什么，它包含的都是Object。

但这种带通配符的List仅__表示他是所有泛型List的父类__，并不能把元素加入到其中，例如下面的代码会引起编译错误。

    List<?> c = new ArrayList<String>();
    c.add(new Object());

因为无法确定c集合中元素的类型，所以不能向其中添加对象，唯一的例外是null，它是所有引用类型的实例

另一方面，程序可以调用get()方法来返回List<?>集合指定索引处的元素，其返回值是一个未知类型，但可以肯定的是，它总是一个Object。因此，把get()的返回值赋给一个Object类型的变量，或者放在任何希望是Object的地方都是可以的

###设定类型通配符的上限
也就是加上一个限定，比如

    //它表示所有Shape泛型List的父类
    List<? extends Shape>
即List<? extends Shape>可以表示List<Circle>、List<Rectangle>的父类，这样可以简化代码。这里简略描述。

###设定类型形参的上限

Java泛型可以在定义类型形参时设定上限，用于表示传给该类型形参的实际类型要么是该上限类型，要么是该上限类型的子类

    public class Apple<T extends Number> {
        T col;
        public static void main(String[] args) {
            Apple<Integer> ai = new Apple<>();
            Apple<Double> ad = new Apple<>();
            //String不是Number的子类型，所以引起编译错误
            Apple<String> as = new Apple<>();
        }
    }

上面这个程序定义了一个Apple泛型类，使用Apple类时为T形参传入的实际类型参数只能是Number或Number类的子类。在一种更极端的情况下，程序需要为类型形参设定多个上限（至多一个父类上限，可以有多个接口上限），表明该类型形参必须是其父类或是父类的子类，并且实现多个上限接口。如下代码所示。

    //表明T类型必须是Number类或其子类，并必须实现java.io.Serializable接口
    public class Apple<T extends Number & java.io.Serializable> {
        ...
    }

##泛型方法

这一块内容有点杂，看书上的吧，这里就不做笔记了

###“菱形”语法与泛型构造器

```java
class MyClass<E> {
    public <T> MyClass(T t) {
        System.out.println("t参数的类型为：" + t);
    }
}

public class GenericDiamondTest {

    public static void main(String[] args) {
        MyClass<String> mc1 = new MyClass<>(5);
        MyClass<String> mc2 = new <Integer> MyClass<String>(5);
        //MyClass<String> mc2 = new <Integer> MyClass<>(5);     
    }

}
```

如果程序显式指定了泛型构造器中声明的类型形参的实际类型，则不可以使用“菱形”语法

###设定通配符下限

```java
import java.util.*;

public class MyUtils {
    
    public static <T> T copy(Collection<? super T> dest
            , Collection<T> src)
    {
        T last = null;
        for (T ele : src) {
            last = ele;
            dest.add(ele);
        }
        return last;
    }

    public static void main(String[] args) {
        
        List<Number> ln = new ArrayList<>();
        List<Integer> li = new ArrayList<>();
        li.add(5);
        Integer last = copy(ln, li);
        System.out.println(ln);

    }

}
```

通配符的下限：<? super Type>，这个通配符表示它必须是Type本身，或是Type的父类


```java
import java.util.*;

public class ErasureTest2 {
    public static void main(String[] args) {
        List<Integer> li = new ArrayList<>();
        li.add(6);
        li.add(15);
        //丢失list集合里元素的类型信息
        //这是典型的擦除
        List list = li;
        //Java又允许直接把List对象赋给一个List<Type>类型的变量
        //但对list变量实际上引用的是List<Integer>集合，所以下面“输出”代码会出现异常
        List<String> ls = list;
        System.out.println(ls.get(0));
    }
}
```

##泛型与数组

这一块要用的时候再看

#9.异常处理

##异常处理机制

###使用try...catch捕获异常
把系统的业务实现代码放在try块中定义，所有的异常处理逻辑放在catch块中进行处理。下面是Java异常处理机制的语法结构

    try
    {
        //业务实现代码
    }
    catch (Exception e)
    {
        alert 输入不合法
        goto retry
    }

当Java运行时环境收到异常对象时，会从上到下，依次寻找能处理该异常对象的catch块，如果找到合适的catch块，则把该异常对象交给该catch块处理，这个过程被称为捕获（catch）异常；如果找不到合适的catch块，则运行时环境终止，Java程序也将退出。

try块和catch块后面的{}__绝不能省略__

Java把所有的非正常情况分成两种：异常（Exception）和错误（Error）。不要试图使用catch块来捕获Error对象

注意：异常捕获时，一定要记住先捕获小异常，再捕获大异常

###多异常捕获

注意如下两个地方

- 捕获多种类型的异常时，多种异常类型之间用竖线（|）隔开
- 捕获多种类型的异常时，异常变量有隐式的final修饰，因此程序不能对异常变量重新赋值

```java
public class MultiExceptionTest {

    public static void main(String[] args) {
        
        try {
            int a = Integer.parseInt(args[0]);
            int b = Integer.parseInt(args[1]);
            int c = a / b;
            System.out.println("结果为：" + c);
        }
        catch (IndexOutOfBoundsException|NumberFormatException
                |ArithmeticException ie)
        {
            System.out.println("程序发生了数组越界、数字格式异常、算数异常之一");
            //捕获多异常时，异常变量默认有final修饰
            //所以下面代码有错
            //ie = new ArithmeticException("test");
        }
        catch (Exception e)
        {
            System.out.println("未知异常");
            //捕获一种类型的异常时，异常变量没有final修饰
            //下面代码正确
            e = new RuntimeException("test");
        }

    }

}

```

###访问异常信息

通过异常参数，像上面程序中的“e”，“ie”，可以获得异常的相关信息

- getMessage()
- printStackTrace()
- printStackTrace(PrintStream s)
- getStackTrace()

###使用finally回收资源
有些时候，程序在try块里打开了一些物理资源（例如数据库链接、网络连接和磁盘文件等），这些物理资源都必须显示回收

__注意：__Java的垃圾回收机制不会回收任何物理资源，垃圾回收机制只能回收堆内存中对象所占用的内存

finally块总会被执行

完整的Java异常处理语法结构如下：

    try
    {
        //业务实现代码
        ...
    }
    catch (SubException e)
    {
        //异常处理块1
        ...
    }
    catch (SubException2 e)
    {
        //异常处理块2
        ...
    }
    ...
    finally
    {
        //资源回收块
        ...
    }

try块是必须的；catch和finally都是可选的，但catch和finally至少出现其一；finally必须位于所有的catch块之后

```java
import java.io.*;

public class FinallyTest {

    public static void main(String[] args) {
        
        FileInputStream fis = null;
        try
        {
            fis = new FileInputStream("a.txt");
        }
        catch (IOException ioe)
        {
            System.out.println(ioe.getMessage());
            //强制返回，但一定会先执行finally块里的代码
            return;
            //使用exit退出虚拟机
            //System.exit(1);
        }
        finally
        {
            if(fis != null) {
                try
                {
                    fis.close();
                }
                catch (IOException ioe)
                {
                    ioe.printStackTrace();
                }
            }
            System.out.println("执行finally块里的资源回收！");
        }

    }

}
```

虽然return语句也强制方法结束，但一定会先执行finally块里的代码

运行结果如下：

    a.txt (系统找不到指定的文件。)
    执行finally块里的资源回收！

如果注释掉return，取消注释System.exit(1)，结果如下

    a.txt (系统找不到指定的文件。)

这表明finally块没有被执行。如果在异常处理代码中使用System.exit(1)来退出虚拟机，则finally块将失去执行的机会

__注意：__除非在try块、catch块中调用了退出虚拟机的方法，否则不管在try块、catch块中执行怎样的代码，出现怎样的情况，异常处理的finally块总会被执行。在通常情况下，不要在finally块中使用如return或throw等导致方法终止的语句，一旦在finally块中使用了return或throw语句，将会导致try块、catch块中的return、throw语句失效

```java
public class FinallyFlowTest {

    public static void main(String[] args) 
        throws Exception
    {
        boolean a = test();
        System.out.println(a);
    }
    
    public static boolean test() {
        try
        {
            return true;
        }
        finally
        {
            return false;
        }
    }

}
```

运行结果为：false

当Java程序执行try块、catch块时遇到了return或throw语句，这两个语句都会导致该方法立即结束，但是系统执行这两个语句并不会结束该方法，而是去寻找该异常处理流程中是否包含finally块，如果没有finally块，程序立即执行return或throw语句，方法终止；如果有finally块，系统立即执行finally块，只有当finally块执行完成后，系统才会再次跳回来执行try块、catch块里的return或throw语句；如果finally块里也使用了return或throw等导致方法终止的语句，finally块已经终止了方法，系统将不会跳回去执行try块、catch块里的任何代码

__注意：__尽量避免在finally块里使用return或throw等导致方法终止的语句，否则可能会出现一些很奇怪的情况

###自动关闭资源的try语句
允许在try关键字后紧跟一对圆括号，可以声明、初始化一个或多个资源（那些必须在程序结束时显式关闭的资源，如数据库连接、网络连接等），这些资源实现类必须实现AutoCloseable或Closeable接口，实现这两个接口就必须实现close()方法

Closeable接口里的close()方法声明抛出了IOException，AutoCloseable的close()声明抛出了Exception

看下面的小例子

```java
import java.io.*;

public class AutoCloseTest {

    public static void main(String[] args) 
        throws IOException
    {
        try (
            //声明、初始化两个可关闭的资源
            //try语句会自动关闭这两个资源
            BufferedReader br = new BufferedReader (
                    new FileReader("AutoCloseTest.java"));
            PrintStream ps = new PrintStream(
                    new FileOutputStream("a.txt"))
                )
        {
            System.out.println(br.readLine());
            ps.print("啦啦啦");
        }

    }

}
```

自动关闭资源的try语句相当于包含了隐式的finally块，因此这个try语句可以既没有catch块，也没有finally块。当然，如有需要，也可加上

##Checked异常和Runtime异常体系

###使用throws声明抛出异常
使用throws声明抛出异常的思路是：当前方法不知道如何处理这种类型的异常，该异常应该由上一级调用者处理；如果main方法也不知道如何处理这种类型的异常，也可以使用throws声明抛出异常，该异常将交给JVM处理。JVM对异常的处理方法是，打印异常的跟踪栈信息，并终止程序运行。

throws声明抛出只能在方法签名中使用，throws可以声明抛出多个异常类，多个异常类之间用逗号隔开，语法如下：

    throws ExceptionClass1 , ExceptionClass2...

一旦使用throws语句声明抛出该异常，程序就无须使用try...catch块来捕获该异常了

使用Checked异常有很多不便（看书或查资料看看好了，这里就不详细写了），所以在大部分时候推荐使用Runtime异常，尤其当程序需要自行抛出异常时，使用Runtime异常将更加简洁。C#、Ruby、Python等语言都没有所谓的Checked异常，所有的异常都是Runtime异常。（这里就不详细描述这两者的优缺点了）

##使用throw抛出异常
throw语句抛出的不是异常类，而是一个异常实例，而且每次只能抛出一个异常实例

    throw ExceptionInstance;

```java
public class ThrowTest {
    public static void main(String[] args) {
        try
        {
            throwChecked(-3);
        }
        catch (Exception e)
        {
            System.out.println(e.getMessage());
        }
        throwRuntime(3);
    }
    
    public static void throwChecked(int a) throws Exception
    {
        if (a > 0) {
            throw new Exception("a的值大于0，不符合要求");
        }
    }
    
    public static void throwRuntime(int a)
    {
        if (a > 0)
        {
            throw new RuntimeException("a的值大于0，不符合要求");
        }
    }
}
```


###自定义异常类
用户自定义异常都应该继承Exception基类，如果希望自定义Runtime异常，则应该继承RuntimeException基类。定义异常类时通常需要提供两个构造器：一个是无参数的构造器，另一个是带一个字符串参数的构造器，这个字符串将作为该异常对象的描述信息（也就是异常对象的getMessage()方法的返回值）

###catch和throw同时使用
实现通过多个方法协作处理同一个异常的情形，可以在catch块中结合throw语句来完成

```java
public class AuctionTest {
    
    private double initPrice = 30.0;
    
    public void bid(String bidPrice) throws AuctionException
    {
        double d = 0.0;
        try
        {
            d = Double.parseDouble(bidPrice);
        }
        catch (Exception e)
        {
            e.printStackTrace();
            throw new AuctionException("竞拍价必须是数值，不能包含其他字符");
        }
        if (initPrice > d)
        {
            throw new AuctionException("竞拍价比起拍价低，不允许竞拍！");
        }
        initPrice = d;
    }

    public static void main(String[] args) {
        AuctionTest at = new AuctionTest();
        try
        {
            at.bid("df");
        }
        catch (AuctionException ae)
        {
            System.err.println(ae.getMessage());
        }

    }

}
```

catch块捕获到异常后，系统打印了该异常的跟踪栈信息，接着抛出一个AuctionException异常，通知该方法的调用者再次处理该AuctionException异常。所以程序中的main方法，也就是bid()方法调用者还可以再次不过AuctionException异常，并将该异常的详细描述信息输出到标准错误输出

###异常链
这种把捕获一个异常然后抛出另一个异常，并把原始异常信息保存下来是一种典型的链式处理，也被称为异常链

##异常处理规则
看书好了

#10.Annotation（注解）

Java8规定，如果接口中只有一个抽象方法（可以包含多个默认方法或多个static方法），该接口就是函数式接口


（略）有空再看，这部分内容先放着

#11.输入/输出

一些基本操作

```java
import java.io.*;

public class FileTest {
    public static void main(String[] args)
        throws IOException 
    {
        //以当前路径创建一个File对象
        File file = new File(".");
        //直接获取文件名，输出一点
        System.out.println(file.getName());
        //获取相对路径的父路径可能出错，下面代码输出null
        System.out.println(file.getParent());
        //获取绝对路径
        System.out.println(file.getAbsoluteFile());
        //获取上一级路径
        System.out.println(file.getAbsoluteFile().getParent());
        //在当前路径下创建一个临时文件
        File tmpFile = File.createTempFile("aaa", ".txt", file);
        //指定当JVM退出时删除该文件
        tmpFile.deleteOnExit();
        //以系统当前时间作为新文件名来创建新文件
        File newFile = new File(System.currentTimeMillis() + "");
        System.out.println("newFile对象是否存在：" + newFile.exists());
        //以指定newFile对象来创建一个文件
        newFile.createNewFile();
        //以newFile对象来创建一个目录，因为newFile已经存在
        //所以下面方法返回false，即无法创建该目录
        System.out.println("newFile能否创建目录：" + newFile.mkdir());
        //使用list()方法列出当前路径下的所有文件和路径
        String[] fileList = file.list();
        System.out.println("====当前路径下所有文件和路径如下====");
        for (String fileName : fileList) {
            System.out.println(fileName);
        }
        //listRoots()静态方法列出所有的磁盘根路径
        File[] roots = File.listRoots();
        System.out.println("====系统所有根路径如下====");
        for (File root : roots) {
            System.out.println(root);
        }

    }
}
```

###文件过滤器
File类的list()方法中可以接受一个FilenameFilter参数。FilenameFilter接口里包含了一个accept(File dir, String name)方法，该方法将依次对指定File的所有子目录或者文件进行迭代，如果该方法返回true，则list()方法会列出该子目录或者文件

```java
import java.io.*;

public class FilenameFilterTest {

    public static void main(String[] args) {
        File file = new File(".");
        //文件名以.java结尾，或者文件对应一个路径，则返回true
        String[] nameList = file.list((dir, name) -> name.endsWith(".java")
                || new File(name).isDirectory());
        for (String name : nameList) {
            System.out.println(name);
        }

    }

}
```

当前路径下所有的*.java文件以及文件夹被列出

Lambda表达式实现了accept()方法，也就是指定自己的规则，指定哪些文件应该由list()方法列出

FilenameFilter接口内只有一个抽象方法，因此该接口也是一个函数式接口，可使用Lambda表达式创建实现该接口的对象

##理解Java的IO流

##字节流和字符流

###InputStream和Reader

```java
package f106.CJ_11;

import java.io.*;

public class FileInputStreamTest {
    public static void main(String[] args) throws IOException
    {
        FileInputStream fis = new FileInputStream(
                "E:\\Crazy_Java\\CJ_11\\src\\f106\\CJ_11\\FileInputStreamTest.java");
        byte[] bbuf = new byte[1024];
        int hasRead = 0;
        while ((hasRead = fis.read(bbuf)) > 0 ) {
            System.out.print(new String(bbuf, 0, hasRead));
        }
        fis.close();
    }
}
```

实际上该Java源文件的长度还不到1024字节，也就是说这是一次读取。程序里打开的文件IO资源不属于内存里的资源，垃圾回收机制无法回收该资源，所以应该显式关闭文件IO资源

输出结果：

    package f106.CJ_11;
    import java.io.*;
    public class FileInputStreamTest {
        public static void main(String[] args) throws IOException
        {
            FileInputStream fis = new FileInputStream(
                    "E:\\Crazy_Java\\CJ_11\\src\\f106\\CJ_11\\FileInputStreamTest.java");
            byte[] bbuf = new byte[1024];
            int hasRead = 0;
            while ((hasRead = fis.read(bbuf)) > 0 ) {
                System.out.print(new String(bbuf, 0, hasRead));
            }
            fis.close();
        }
    }


```java
package f106.CJ_11;

import java.io.*;

public class FileReaderTest {

    public static void main(String[] args) 
        throws IOException
    {
        try (
            FileReader fr = new FileReader("E:\\Crazy_Java\\CJ_11\\src\\f106\\CJ_11\\FileReaderTest.java"))
        {
            char[] cbuf = new char[32];
            int hasRead = 0;
            while ((hasRead = fr.read(cbuf)) > 0) {
                System.out.println(new String(cbuf, 0, hasRead));
            }
        }
        catch (IOException ex)
        {
            ex.printStackTrace();
        }
    }
}
```

这里讲字符数组的长度改为32，这意味着要多次调用read()方法才可以完全读取输入流的全部数据。这里还使用了能自动关闭资源的try语句来关闭文件输入流，这样可以保证输入流一定会被关闭

输出结果：

    package f106.CJ_11;
    import ja
    va.io.*;
    public class FileRea
    derTest {
        public static void
     main(String[] args) 
            throws 
    IOException
        {
            try (
                Fil
    eReader fr = new FileReader("E:\
    \Crazy_Java\\CJ_11\\src\\f106\\C
    J_11\\FileReaderTest.java"))
            
    {
                char[] cbuf = new char[32]
    ;
                int hasRead = 0;
                while
     ((hasRead = fr.read(cbuf)) > 0)
     {
                    System.out.println(new S
    tring(cbuf, 0, hasRead));
                }
            }
            catch (IOException ex)
            {
                ex.printStackTrace();
        
        }
        }
    }

###OutputStream和Writer

```java
package f106.CJ_11;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.FileOutputStream;


public class FileOutputStreamTest {

    public static void main(String[] args) {
        try(
            //创建字节输入流
            FileInputStream fis = new FileInputStream(
                    "E:\\Crazy_Java\\CJ_11\\src\\f106\\CJ_11\\FileOutputStreamTest.java");
            //创建字节输出流
            FileOutputStream fos = new FileOutputStream("newFile.txt"))
        {
            byte[] bbuf = new byte[32];
            int hasRead = 0;
            //循环从输入流中读取数据
            while ((hasRead = fis.read(bbuf)) > 0 ) {
                //读多少，写多少
                fos.write(bbuf, 0, hasRead);
            }
        }
        catch (IOException ioe)
        {
            ioe.printStackTrace();
        }

    }

}
```

注意：使用Java的IO流执行输出时，不要忘记关闭输出流，关闭输出流出了可以保证流的物理资源被回收外，可能还可以将输出流缓冲区中的数据flush到物理节点里（因为在执行close()方法之前，自动执行输出流的flush()方法）。Java的很多输出流默认都提供了缓冲功能，只要正常关闭所有的输出流即可保证程序正常

用Writer直接输出字符串内容会有更好的效果
```java
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterTest {

    public static void main(String[] args) {
        try (
            FileWriter fw = new FileWriter("poem.txt"))
        {
            // \r\n是Windows平台下的换行符
            // 如果是UNIX/LINUX/BSD等平台，则使用\n作为换行符
            fw.write("好好学习\r\n");
            fw.write("天天向上\r\n");
        }
        catch (IOException ioe)
        {
            ioe.printStackTrace();
        }
        
    }
}
```

##输入/输出流体系

如果希望简化编程，就需要借助于处理流了

使用处理流的典型思路是：使用处理流来包装节点流，程序通过处理流来执行输入/输出功能，让节点流与底层I/O设备、文件交互。

只要流的构造器参数不是一个物理节点，而是已经存在的流，那么这种流就一定是处理流；而所有的节点流都是直接以物理IO节点作为构造器参数的。

使用处理流的优势：1.操作更简单；2.效率更高

下面程序使用PrintStream处理流来包装OutputStream

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.PrintStream;

public class PrintStreamTest {

    public static void main(String[] args) {
        try (
            FileOutputStream fos = new FileOutputStream("test.txt");
            PrintStream ps = new PrintStream(fos))
        {
            ps.println("普通字符串");
            ps.println(new PrintStreamTest());
        }
        catch (IOException ioe)
        {
            ioe.printStackTrace();
        }
        
    }

}
```

很强大有木有！！！直接输出到文本就可以啦！！非常方便！

__注意：__在使用处理流包装了底层节点流之后，关闭输入/输出流资源时，只要关闭最上层的处理流即可。关闭最上层的处理流是，系统会自动关闭被该处理流包装的节点流

###输入/输出流体系

通常有一个规则：如果进行输入/输出的内容是文本内容，则应该考虑使用字符流；如果是二进制内容，则考虑字节流

字符流可以使用字符串作为物理节点，用于实现从字符串读取内容，或将内容写入字符串（用StringBuffer充当字符串）的功能。如下所示

```java
import java.io.IOException;
import java.io.StringReader;
import java.io.StringWriter;

public class StringNodeTest {

    public static void main(String[] args) {
        String src = "好好学习\n"
                + "天天向上\n";
        char[] buffer = new char[32];
        int hasRead = 0;
        try (StringReader sr = new StringReader(src))
        {
            while ((hasRead = sr.read(buffer)) > 0 ) {
                System.out.print(new String(buffer, 0, hasRead));
            }
        }
        catch (IOException ioe)
        {
            ioe.printStackTrace();
        }
        try (
            //创建StringWriter时，实际上是以一个StringBuffer作为输出节点
            StringWriter sw = new StringWriter())
        {
            sw.write("天龙八部\n");
            sw.write("射雕三部曲\n");
            System.out.println("----下面是sw字符串节点里的内容----");
            System.out.println(sw.toString());
        }
        catch (IOException ex)
        {
            ex.printStackTrace();
        }

    }

}
```

输出结果：

    好好学习
    天天向上
    ----下面是sw字符串节点里的内容----
    天龙八部
    射雕三部曲

上面程序与FileReader和FileWriter的程序基本相似。由于String是不可变的字符串对象，所以使用StringBuffer作为输出节点

###转换流

两个转换流，用于实现将字节流转换成字符流

- InputStreamReader将字节输入流转换成字符输入流
- OutputStreamReader将字节输出流转换成字符输出流

没有把字符流转换成字节流的转换流。看看字符流与字节流的差别：字节流比字符流的使用范围更广，但字符流比字节流操作__更方便__。

看下面这个例子。Java使用System.in代表标准输入，即键盘输入，但这个标准输入流是InputStream类的实例，而且键盘输入内容都是文本内容，所以可以用InputStreamReader将其转换成字符输入流；普通的Reader读取输入内容是依然不太方便，可以将普通的Reader再次包装成BufferedReader，利用BufferedReader的readLine()方法可以一次读取一行内容。

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class KeyinTest {

    public static void main(String[] args) {
        try (
            InputStreamReader reader = new InputStreamReader(System.in);
            BufferedReader br = new BufferedReader(reader))
        {
            String line = null;
            while ((line = br.readLine()) != null) {
                if (line.equals("exit")) {
                    System.exit(1);
                }
                System.out.println("输入内容为：" + line);
            }
        }
        catch (IOException ioe)
        {
            ioe.printStackTrace();
        }
    }
}
```

BufferedReader流具有缓冲功能，它可以一次读取一行文本————以换行符为标志

###推回输入流
在输入/输出流体系中，有两个特殊的流与众不同，就是PushbackInputStream和PushbackReader。这两个输入流都带有一个推回缓冲区，当调用unread()时，系统将会把指定数组的内容推回到该缓冲区里，而推回输入流每次调用read()方法时总是先从推回缓冲区读取，只有当完全读完后，但还没有装满read()所需的数组时才会从原输入流中读取。当创建一个PushbackInputStream和PushbackReader时需要指定推回缓冲区的大小，默认的推回缓冲区的长度为1。如果推回到推回缓冲区的内容超出了其大小，则会引发Pushback buffer overflow的IOException异常

下面程序打印出“new PushbackReader”字符串之前的内容

```java
package f106.CJ_11;
import java.io.FileReader;
import java.io.IOException;
import java.io.PushbackReader;
public class PushbackTest {
    public static void main(String[] args) {
        try(
            PushbackReader pr = new PushbackReader(new FileReader(
                    "E:\\Crazy_Java\\CJ_11\\src\\f106\\CJ_11\\PushbackTest.java"), 64))
        {
            char[] buf = new char[32];
            String lastContent = "";
            int hasRead = 0;
            while ((hasRead = pr.read(buf)) > 0) {
                String content = new String(buf, 0, hasRead);
                int targetIndex = 0;
                if ((targetIndex = (lastContent + content)
                        .indexOf("new PushbackReader")) > 0)
                {
                    //将本次内容和上次内容一起推回缓冲区
                    pr.unread((lastContent + content).toCharArray());
                    if (targetIndex > 32)
                    {
                        buf = new char[targetIndex];
                    }
                    pr.read(buf, 0, targetIndex);
                    System.out.print(new String(buf, 0, targetIndex));
                    System.exit(0);
                }
                else
                {
                    System.out.print(lastContent);
                    lastContent = content;
                }
            }
        }
        catch (IOException ioe)
        {
            ioe.printStackTrace();
        }
        
    }

}
```

输出结果：

    package f106.CJ_11;
    import java.io.FileReader;
    import java.io.IOException;
    import java.io.PushbackReader;
    public class PushbackTest {
        public static void main(String[] args) {
            try(
                PushbackReader pr = 

##重定向标准输入/输出
Java的标准输入/输出分别通过System.in和System.out来代表，在默认情况下他们分别代表键盘和显示器；当程序通过System.in来获取输入时，实际上是从键盘读取输入；当程序试图通过System.out执行输出时，程序总是输出到屏幕。在System类里提供了如下三个重定向标准输入/输出的方法

- static void setErr(PrintStream err)：重定向“标准”错误输出流
- static void setIn(InputStream in)：重定向“标准”输入流
- static void setOut(PrintStream out)：重定向“标准”输出流

下面程序通过重定向标准输出流，将System.out的输出重定向到文件输出

```java
package f107.CJ_11;

import java.io.*;

public class RedirectOut {

    public static void main(String[] args) {
        try(
            //一次性创建PrintStream输出流
            PrintStream ps = new PrintStream(new FileOutputStream("out.txt")))
        {
            //将标准输出重定向到ps输出流
            System.setOut(ps);
            System.out.println("普通字符串");
            System.out.println(new RedirectOut());
        } 
        catch (IOException ex) {            
            ex.printStackTrace();
        }

    }

}
```

下面程序重定向标准输入，从而将System.in重定向到指定文件

```java
package f107.CJ_11;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Scanner;

public class RedirectIn {

    public static void main(String[] args) {
        try(FileInputStream fis = new FileInputStream("src/f107/CJ_11/RedirectIn.java"))
        {
            //将标准输入重定向到fis输入流
            System.setIn(fis);
            //使用System.in创建Scanner对象，用于获取标准输入
            Scanner sc = new Scanner(System.in);
            //只把回车作为分隔符
            sc.useDelimiter("\n");
            while(sc.hasNext()) {
                System.out.println("键盘输入的内容是：" + sc.next());
            }
        }
        catch (IOException ex)
        {
            ex.printStackTrace();
        }

    }

}
```


##RandomAccessFile
RandomAccessFile支持“随机访问”的方式，程序可以直接跳转到文件的任意地方来读写数据。Random除了有“随机”的意思外，还有“任意”的意思。国内翻译不好，所以尽量看原版IT技术文章

下面程序使用了RandomAccessFile俩访问指定的中间部分数据

```java
package f107.CJ_11;

import java.io.*;

public class RandomAccessFileTest {

    public static void main(String[] args) {
        try(
            RandomAccessFile raf = new RandomAccessFile(
                    "src/f107/CJ_11/RandomAccessFileTest.java" , "r"))
        {
            //获取RandomAccessFile对象文件指针的位置，初始位置是0
            System.out.println("RandomAccessFile的文件指针的初始位置："
                    + raf.getFilePointer());
            //移动raf的文件记录指针的位置，从300字节处开始读
            raf.seek(300);
            byte[] bbuf = new byte[1024];
            int hasRead = 0;
            while ((hasRead = raf.read(bbuf)) > 0) {
                System.out.print(new String(bbuf, 0, hasRead));
            }
        }
        catch (IOException ex)
        {
            ex.printStackTrace();
        }

    }

}
```


下面程序示范了如何向指定文件后追加内容。为了追加内容，程序应该先将记录指针移动到文件最后，然后开始向文件中输出内容
```java
package f107.CJ_11;

import java.io.IOException;
import java.io.RandomAccessFile;

public class AppendContent {

    public static void main(String[] args) {
        try(
            RandomAccessFile raf = new RandomAccessFile("out.txt" , "rw"))
        {
            //将记录指针移动到out.txt文件的最后
            raf.seek(raf.length());
            raf.write("追加的内容！\r\n".getBytes());
        }
        catch (IOException ex)
        {
            ex.printStackTrace();
        }

    }

}
```

__注意：__RandomAccessFile依然不能向文件的指定文职插入内容，如果直接将文件记录指针移动到中间某位置开始输出，则新输出的内容会覆盖文件中原有的内容。如果需要向指定位置插入内容，程序需要先把插入点后面的内容读入缓冲区，等把需要插入的数据写入文件后，再将缓冲区的内容追加到文件后面

下面程序实现了向指定文件、指定位置插入内容的功能

```java
package f107.CJ_11;

import java.io.*;

public class InsertContent {

    public static void insert(String fileName, long pos
            , String insertContent) throws IOException
    {
        File tmp = File.createTempFile("tmp", null);
        tmp.deleteOnExit();
        try(
            RandomAccessFile raf = new RandomAccessFile(fileName , "rw");
            //使用临时文件来保存插入后的数据
            FileOutputStream tmpOut = new FileOutputStream(tmp);
            FileInputStream tmpIn = new FileInputStream(tmp))
        {
            raf.seek(pos);
            //---下面代码将插入点后的内容读入临时文件中保存---
            byte[] bbuf = new byte[64];
            int hasRead = 0;
            while ((hasRead = raf.read(bbuf)) > 0 )
            {
                //将读取的数据写入临时文件中
                tmpOut.write(bbuf, 0, hasRead);
            }
            //---下面代码用于插入内容---
            //把文件记录指针重新定位到pos位置
            raf.seek(pos);
            //追加需要插入的内容
            raf.write(insertContent.getBytes());
            //追加临时文件中的内容
            while ((hasRead = tmpIn.read(bbuf)) > 0) {
                raf.write(bbuf, 0, hasRead);
            }
        }
    }
    
    public static void main(String[] args) 
        throws IOException
    {
        insert("InsertContent.java" , 45 , "插入的内容\r\n");        
    }

}
```

##NIO.2
NIO.2提供了Files、Paths两个工具类，其中Files包含了大量静态的工具方法来操作文件，Paths则包含了两个返回Path的静态方法

```java
package f107.CJ_11;

import java.nio.file.Path;
import java.nio.file.Paths;

public class PathTest {

    public static void main(String[] args) 
        throws Exception
    {
        //以当前路径创建Path对象
        Path path = Paths.get(".");
        System.out.println("path里包含的路径数量："
                + path.getNameCount());
        System.out.println("path的根路径：" + path.getRoot());
        //获取path对应的绝对路径
        Path absolutePath = path.toAbsolutePath();
        System.out.println("绝对路径为：" + absolutePath);
        //获取绝对路径的根路径
        System.out.println("absolutePath的根路径："
                + absolutePath.getRoot());
        //获取绝对路径所包含的路径数量
        System.out.println("absolutePath里包含的路径数量："
                + absolutePath.getNameCount());
        System.out.println(absolutePath.getName(0));
        //以多个String来构建Path对象
        Path path2 = Paths.get("E:" , "publish" , "codes");
        System.out.println(path2);
        System.out.println(path2.getNameCount());

    }

}
```

输出结果：

    path里包含的路径数量：1
    path的根路径：null
    绝对路径为：E:\Crazy_Java\CJ_11\.
    absolutePath的根路径：E:\
    absolutePath里包含的路径数量：3
    Crazy_Java
    E:\publish\codes
    2

Files是一个操作文件的工具类，它提供了大量便捷的工具方法，下面程序简单示范了Files类的用法

```java
package f107.CJ_11;
import java.io.FileOutputStream;
import java.nio.charset.Charset;
import java.nio.file.FileStore;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;
public class FilesTest {
    public static void main(String[] args) 
        throws Exception
    {
        //复制文件
        Files.copy(Paths.get("src/f107/CJ_11/FilesTest.java"), new FileOutputStream("a.txt"));
        //判断FilesTest.java文件是否为隐藏文件
        System.out.println("FilesTest.java是否为隐藏文件："
                + Files.isHidden(Paths.get("src/f107/CJ_11/FilesTest.java")));
        //一次性读取FilesTest.java文件的所有行
        List<String> lines = Files.readAllLines(Paths
                .get("src/f107/CJ_11/FilesTest.java"), Charset.forName("gbk"));
        System.out.println(lines);
        //判断指定文件的大小
        System.out.println("FileTest.java的大小为："
                + Files.size(Paths.get("src/f107/CJ_11/FilesTest.java")));
        List<String> poem = new ArrayList<>();
        poem.add("好好学习");
        poem.add("天天向上");
        //直接将多个字符串内容写入指定文件中
        Files.write(Paths.get("poem.txt"), poem, Charset.forName("gbk"));
        //使用Java8新增的Stream API列出当前目录下所有文件和子目录
        Files.list(Paths.get(".")).forEach(path -> System.out.println(path));
        //使用Java8新增的Stream API读取文件内容
        Files.lines(Paths.get("src/f107/CJ_11/FilesTest.java") , Charset.forName("gbk"))
            .forEach(line -> System.out.println(line));
        FileStore cStore = Files.getFileStore(Paths.get("C:"));
        //判断C盘的总空间、可用空间
        System.out.println("C:共有空间：" + cStore.getTotalSpace());
        System.out.println("C:可用空间：" + cStore.getUsableSpace());
    }
}
```

输出结果：

    FilesTest.java是否为隐藏文件：false
    [package f107.CJ_11;, import java.io.FileOutputStream;, import java.nio.charset.Charset;, import java.nio.file.FileStore;, import java.nio.file.Files;, import java.nio.file.Paths;, import java.util.ArrayList;, import java.util.List;, public class FilesTest {,     public static void main(String[] args) ,        throws Exception,   {,      //复制文件,         Files.copy(Paths.get("src/f107/CJ_11/FilesTest.java"), new FileOutputStream("a.txt"));,         //判断FilesTest.java文件是否为隐藏文件,        System.out.println("FilesTest.java是否为隐藏文件：",                + Files.isHidden(Paths.get("src/f107/CJ_11/FilesTest.java")));,         //一次性读取FilesTest.java文件的所有行,        List<String> lines = Files.readAllLines(Paths,              .get("src/f107/CJ_11/FilesTest.java"), Charset.forName("gbk"));,        System.out.println(lines);,         //判断指定文件的大小,        System.out.println("FileTest.java的大小为：",                + Files.size(Paths.get("src/f107/CJ_11/FilesTest.java")));,         List<String> poem = new ArrayList<>();,         poem.add("好好学习");,      poem.add("天天向上");,      //直接将多个字符串内容写入指定文件中,        Files.write(Paths.get("poem.txt"), poem, Charset.forName("gbk"));,      //使用Java8新增的Stream API列出当前目录下所有文件和子目录,      Files.list(Paths.get(".")).forEach(path -> System.out.println(path));,      //使用Java8新增的Stream API读取文件内容,       Files.lines(Paths.get("src/f107/CJ_11/FilesTest.java") , Charset.forName("gbk")),           .forEach(line -> System.out.println(line));,        FileStore cStore = Files.getFileStore(Paths.get("C:"));,        //判断C盘的总空间、可用空间,        System.out.println("C:共有空间：" + cStore.getTotalSpace());,        System.out.println("C:可用空间：" + cStore.getUsableSpace());,   }, }]
    FileTest.java的大小为：1654
    .\.classpath
    .\.project
    .\.settings
    .\1507289010027
    .\1507289121871
    .\a.txt
    .\bin
    .\InsertContent.java
    .\newFile.txt
    .\out.txt
    .\poem.txt
    .\src
    .\test.txt
    package f107.CJ_11;
    import java.io.FileOutputStream;
    import java.nio.charset.Charset;
    import java.nio.file.FileStore;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    import java.util.ArrayList;
    import java.util.List;
    public class FilesTest {
        public static void main(String[] args) 
            throws Exception
        {
            //复制文件
            Files.copy(Paths.get("src/f107/CJ_11/FilesTest.java"), new FileOutputStream("a.txt"));
            //判断FilesTest.java文件是否为隐藏文件
            System.out.println("FilesTest.java是否为隐藏文件："
                    + Files.isHidden(Paths.get("src/f107/CJ_11/FilesTest.java")));
            //一次性读取FilesTest.java文件的所有行
            List<String> lines = Files.readAllLines(Paths
                    .get("src/f107/CJ_11/FilesTest.java"), Charset.forName("gbk"));
            System.out.println(lines);
            //判断指定文件的大小
            System.out.println("FileTest.java的大小为："
                    + Files.size(Paths.get("src/f107/CJ_11/FilesTest.java")));
            List<String> poem = new ArrayList<>();
            poem.add("好好学习");
            poem.add("天天向上");
            //直接将多个字符串内容写入指定文件中
            Files.write(Paths.get("poem.txt"), poem, Charset.forName("gbk"));
            //使用Java8新增的Stream API列出当前目录下所有文件和子目录
            Files.list(Paths.get(".")).forEach(path -> System.out.println(path));
            //使用Java8新增的Stream API读取文件内容
            Files.lines(Paths.get("src/f107/CJ_11/FilesTest.java") , Charset.forName("gbk"))
                .forEach(line -> System.out.println(line));
            FileStore cStore = Files.getFileStore(Paths.get("C:"));
            //判断C盘的总空间、可用空间
            System.out.println("C:共有空间：" + cStore.getTotalSpace());
            System.out.println("C:可用空间：" + cStore.getUsableSpace());
        }
    }
    C:共有空间：107375226880
    C:可用空间：78509174784

__注意：__应该熟练掌握Files工具类的用法，它所包含的工具方法可以大大地简化文件IO

###使用FileVisitor遍历文件和目录
###使用WatchService监控文件变化
###访问文件属性
（略，看书吧，感觉不是很重要）


#12.多线程
单线程的程序如同只雇用了一个服务员的餐厅，他必须做完一件事后才可以做下一件事；多线程的程序则如同雇用多个服务员的餐厅，他们可以同时做多件事情

##12.1线程概述

###12.1线程和进程
当一个程序进入内存运行时，即变成一个进程（process）。进程是处于运行过程中的程序，并且具有一定的独立功能，进程是系统进行资源分配和调度的一个独立单位。

进程的三个特征

- 独立性：进程是系统中独立存在的实体，可以拥有自己独立的资源
- 动态性：进程与程序的区别是，程序只是一个静态的指令集合，而进程是一个正在系统中活动的指令集合
- 并发性：多个进程可以在单个处理器上并发执行，多个进程之间不会互相影响

__注意：__并发性（concurrency）和并行性（parallel）是两个概念，并行指在同一个时刻，有多条指令在多个处理器上同时执行；并发指在同一时刻只能有一条指令执行，但多个进程指令被快速轮换执行，使得在宏观上具有多个进程同时执行的效果

多线程使得同一个进程能同时并发处理多个任务。线程（Thread）也被称作轻量级进程（Lightweight Process），线程是进程的执行单元。当进程被初始化后，主线程就被创建了

线程是进程的组成部分，一个进程可以拥有多个线程，一个线程必须有一个父进程。线程可以拥有自己的堆栈、自己的程序计数器和局部变量，但不拥有系统资源线程共享进程所拥有的全部资源。

线程是独立运行的。线程的执行是抢占式的，即当前运行的线程在任何时候都可能被挂起，以便另外一个线程可以运行

__归纳起来可以这么说：__操作系统可以同时执行多个任务，每个任务就是进程；进程可以同时执行多个任务，每个任务就是线程

##线程的创建和启动
线程的作用是完成一定的任务，实际上就是执行一段程序流

###继承Thread类创建线程类
通过继承Thread类来创建并启动多线程的步骤如下

1. 定义Thread类的子类，并重写该类的run()方法，该run()方法的方法体就代表了线程需要完成的任务。因此把run()方法称为线程执行体
2. 创建Thread子类的实例，即创建了线程对象
3. 调用线程对象的start()方法来启动该线程

看下面程序的例子

```java
public class FirstThread extends Thread{

    private int i;
    //重写run()，其方法体就是线程执行体
    public void run() {
        for ( ; i < 100 ; i++) {
            //当线程类继承Thread类时，直接使用this即可获取当前线程
            //Thread对象的getName()返回当前线程的名字
            //因此可以直接调用getName()方法返回当前线程的名字
            System.out.println(getName() + " " + i);
        }
    }
    
    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            //调用Thread的currentThread()方法获取当前线程
            System.out.println(Thread.currentThread().getName()
                    + " " + i + " main里的");
            if (i == 20) {
                //创建并启动第一个线程
                new FirstThread().start();
                //创建并启动第二个线程
                new FirstThread().start();
            }
        }
    }
}
```

输出结果片段：

    Thread-0 0
    Thread-1 39
    main 23 main里的
    Thread-1 40
    Thread-0 1
    Thread-1 41
    main 24 main里的
    Thread-1 42
    Thread-0 2
    Thread-1 43
    main 25 main里的
    Thread-1 44
    Thread-0 3
    Thread-1 45
    main 26 main里的

可以看出两个线程输出的i变量不联系，因为i变量是FirstThread的实例变量，无法共享

__注意：__使用继承Thread类的方法来创建线程类时，多个线程之间无法共享线程类的实例变量

运行时实际上有三个线程。main()方法的方法体代表主线程的线程执行体

上面程序还用到了如下两个方法

- Thread.currentThread()：这是Thread的静态方法，该方法总是返回当前正在执行的线程对象
- getName()：该方法时Thread类的实例方法，该方法返回调用该方法的线程名字



###实现Runnable接口创建线程类
步骤：

1. 定义Runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体
2. 创建Runnable实现类的实例，并以此实例作为Thread的target来创建Thread对象，该Thread对象才是真正的线程对象。

代码如下所示：

    //创建Runnable实现类的对象
    SecondThread st = new SecondThread();
    //以Runnable实现类的对象作为Thread的target来创建Thread对象，即线程对象
    new Thread(st);

也可以在创建Thread对象时为该Thread对象指定一个名字，代码如下所示

    //创建Thread对象时指定target和新线程的名字
    new Thread(st , "新线程1");

提示：Runnable对象仅仅作为Thread对象的target，Runnable实现类里包含的run()方法仅作为线程执行体。而实际的线程对象依然是Thread实例，只是改Thread线程负责执行其target的run()方法

3. 调用线程对象的start()方法来启动该线程

看下面例子

```java
public class SecondThread implements Runnable{
    
    private int i;
    public void run() {
        for ( ; i < 100 ; i++) {
            //当线程类实现Runnable接口时
            //如果想获取当前线程，只能用Thread.currentThread()方法
            System.out.println(Thread.currentThread().getName()
                    + " " + i);
        }
    }

    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName()
                    + " " + i);
            if (i == 20) {
                SecondThread st = new SecondThread();
                //通过new Thread(target, name)方法创建新线程
                new Thread(st , "新线程1").start();
                new Thread(st , "新线程2").start();
            }
        }

    }

}
```

输出结果片段：

    新线程1 0
    main 26
    新线程2 0
    main 27
    新线程1 1
    main 28
    新线程2 2
    main 29
    新线程1 3
    main 30
    新线程2 4
    main 31
    新线程1 5
    main 32
    新线程2 6
    main 33
    新线程1 7
    main 34
    新线程2 8

可以看出两个子线程的i变量是连续的，也就是采用Runnable接口的方式创建的多个线程可以共享线程类的实例变量。这是因为在这种方式下，程序所创建的Runnable对象只是线程的target，而多个线程可以共享同一个target，所以多个线程可以共享同一个线程类的实例变量

###使用Callable和Future创建线程
从Java5开始，Java提供了Callable接口，为Runnable接口的增强版，Callable接口提供了一个call()方法可以作为线程执行体

- call()方法可以有返回值
- call()方法可以声明抛出异常

__注意：__Callable接口有泛型限制，Callable接口里的泛型形参类型与call()方法返回值类型相同。而且Callable接口是函数式接口，因此可使用Lambda表达式创建Callable对象

创建并启动有返回值的线程的步骤如下：

1. 创建Callable接口的实现类，并实现call()方法，该call()方法将作为线程执行体，且有返回值，再创建Callable实现类的实例。从Java8开始，可以直接使用Lambda表达式创建Callable对象
2. 使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对象的call()方法的返回值
3. 使用FutureTask对象作为Thread对象的target创建并启动新线程
4. 调用FutureTask对象的get()方法来获得子线程执行结束后的返回值

```java
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

public class ThirdThread {

    public static void main(String[] args) {
        //创建Callable对象
        ThirdThread rt = new ThirdThread();
        //先使用Lambda表达式创建Callable<Integer>对象
        //使用FutureTask来包装Callable对象
        FutureTask<Integer> task = new FutureTask<Integer>((Callable<Integer>)()->{
            int i = 0;
            for ( ; i < 100 ; i++) {
                System.out.println(Thread.currentThread().getName()
                        + "的循环变量i的值：" + i);
            }
            //call()方法可以有返回值
            return i;
        });
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName()
                    + "的循环变量i的值：" + i);
            if (i == 20) {
                new Thread(task , "有返回值的线程").start();
            }
        }
        try
        {
            //获取线程返回值
            System.out.println("子线程的返回值：" + task.get());
        }
        catch (Exception ex)
        {
            ex.printStackTrace();
        }
        
    }

}
```

上面程序中使用Lambda表达式直接创建了Callable对象，这样就无须先创建Callable实现类，再创建Callable对象。

###创建三种方式对比
实现Runnable接口与实现Callable接口的方式基本相同
对比（略）

一般推荐采用实现Runnable接口、Callable接口的方式来创建多线程

##线程的生命周期
在线程的生命周期中，它要经过新建（New）、就绪（Runnable）、运行（Running）、阻塞（Blocked）和死亡（Dead）5中状态。当线程启动后，它不可能一直“霸占”着CPU独自运行，所以需要在多条线程之间切换，于是线程状态也会多次在运行、就绪之间切换

###新建和就绪状态
当程序使用new关键字创建了一个线程之后，该线程就处于新建状态，仅仅由Java虚拟机为其分配内存，并初始化其成员变量的值，此时的线程对象没有表现出任何线程的动态特征，程序也不会执行线程的线程执行体。

当线程对象调用了start()方法之后，该线程就处于就绪状态，Java虚拟机会为其创建方法调用栈和程序计数器，处于这个状态中的线程并没有开始运行，__只是表示该线程可以运行了__。至于该线程合适开始运行，取决于JVM里线程调度器的调度

__注意：__启动线程使用start()方法，而不是run()!!!调用start()来启动线程，系统会把该run()方法当成线程执行体来处理；但如果直接调用线程对象的run()方法，则run()方法会立即被执行，而且在run()方法返回之前其他线程无法并发执行，即如果直接调用线程对象的run()方法，系统吧线程对象当成一个普通对象，而run()方法也是一个普通方法，而不是线程执行体

看下面例子

```java
public class InvokeRun extends Thread{
    private int i;
    public void run() {
        for ( ; i < 100 ; i++) {
            //直接调用run()方法时，Thread的this.getName()返回的是该对象的名字
            //而不是当前线程的名字
            //使用Thread.currentThread().getName()总是获取当前线程的名字
            System.out.println(Thread.currentThread().getName()
                    + " " + i);
        }
    }

    public static void main(String[] args) {
        for (int i = 0; i < 100; i++) {
            //调用Thread的currentThread()方法获取当前线程
            System.out.println(Thread.currentThread().getName()
                    + " " + i);
            if (i == 20) {
                //直接调用线程对象的run()方法
                //系统会把线程对象当成普通对象，把run()方法当成普通方法
                //所以下面两行代码并不会启动两个线程，而是依次执行两个run()方法
                new InvokeRun().run();
                new InvokeRun().run();
            }
        }

    }

}
```

输出片段：

    ...
    main 19
    main 20
    main 0
    main 1
    ...
    main 98
    main 99
    main 0
    main 1
    ...
    main 98
    main 99
    main 21
    main 22
    ...
    main 98
    main 99

需要指出的是，调用了线程的run()方法之后，该线程已经不再处于新建状态，不要再次调用线程对象的start()方法

__注意：__只能对处于新建状态的线程调用start()方法，否则将引发IllegalThreadStateException异常

调用线程对象的start()方法之后，该线程立即进入就绪状态————就绪状态相当于“等待执行”，但该线程并未真正进入运行状态。这一点可以通过前面的FirstThread或SecondThread证明，都是等待20多的时候子线程才启动的

__提示：__如果希望调用子线程的start()方法后子线程立即开始执行，程序可以使用Thread.sleep(1)来让当前运行的线程睡眠1毫秒，在这1毫秒内CPU不会空闲，他会执行另一个处于就绪状态的线程，这样就可以让子线程立即开始执行

###运行和阻塞状态


###线程死亡
```java
public class StartDead extends Thread{
    private int i;
    public void run() {
        for ( ; i < 300 ; i++) {
            System.out.println(getName() + " " + i);
        }
    }
    public static void main(String[] args) {
        //创建线程对象
        StartDead sd = new StartDead();
        for (int i = 0; i < 300; i++) {
            System.out.println(Thread.currentThread().getName()
                    + " " + i);
            if (i == 20) {
                sd.start();
                //输出true
                System.out.println(sd.isAlive());
            }
            //当线程处于新建、死亡两种状态时，isAlive()方法返回false
            //当i>20时，该线程肯定已经启动过了，如果sd.isAlive()为假时
            //那就是死亡状态了
            if (i > 20 && !sd.isAlive()) {
                //试图再次启动该线程
                sd.start();
            }
        }
    }
}
```

上面程序会发生IllegalThreadStateException异常，这表明处于死亡状态的线程无法再次运行了

__注意：__不要对处于死亡状态的线程调用start()方法，程序只能对新建状态的线程调用start()方法，对新建状态的线程两次调用start()方法也是错误的，这都会引发IllegalThreadStateException异常

##控制线程

###join线程
Thread提供了让一个线程等待另一个线程完成的方法————join()方法。当在某个程序执行流中调用其他线程的join()方法时，调用线程将被阻塞，直到被join()方法加入的join线程执行完为止

```java
public class JoinThread extends Thread{
    
    public JoinThread(String name) {
        super(name);
    }
    
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(getName() + " " + i); 
        }
    }

    public static void main(String[] args) throws Exception
    {
        new JoinThread("新线程").start();
        for (int i = 0; i < 100; i++) {
            if (i == 20) {
                JoinThread jt = new JoinThread("被Join的线程");
                jt.start();
                //main线程调用了jt线程的join()方法
                //main线程必须等jt执行结束才会向下执行
                jt.join();
            }
            System.out.println(Thread.currentThread().getName()
                    + " " + i);
        }

    }

}
```

输出结果片段：

    被Join的线程 95
    被Join的线程 96
    被Join的线程 97
    被Join的线程 98
    被Join的线程 99
    main 20
    main 21
    main 22
    main 23
    main 24

###后台线程
有一种线程，它是在后台运行的，它的任务是为其他的线程提供服务，这种线程被称为“后台线程（Daemon Thread）”，又称为“守护线程”或“精灵线程”。JVM的垃圾回收线程就是典型的后台线程。后台线程有个特征：如果所有的前台线程都死亡了，后台线程会自动死亡。

```java
public class DaemonThread extends Thread{

    //定义后台线程的线程执行体与普通线程没有任何区别
    public void run() {
        for (int i = 0; i < 1000; i++) {
            System.out.println(getName() + " " + i);
        }
    }
    public static void main(String[] args) {
        DaemonThread t = new DaemonThread();
        //将此线程设置成后台线程
        t.setDaemon(true);
        t.start();
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getName()
                    + " " + i);
        }
        // -----程序执行到此结束，前台线程（main线程）结束-----
        // 后台线程也随之结束，不会运行到999！！！！！

    }

}
```

输出结果：

    ...
    Thread-0 45
    main 4
    main 5
    main 6
    main 7
    main 8
    main 9
    Thread-0 46
    Thread-0 47
    Thread-0 48
    Thread-0 49
    Thread-0 50
    Thread-0 51
    Thread-0 52
    Thread-0 53
    Thread-0 54
    Thread-0 55
    (over)

__注意：__前台线程死亡后，JVM会通知后台线程死亡，但从它接收指令到做出响应，需要一定时间，而且__要将某个线程设置为后台线程，必须在线程启动之前设置！！！__

###线程睡眠：sleep
sleep()常用来暂停程序的执行

```java
import java.util.Date;

public class SleepTest {

    public static void main(String[] args) 
        throws Exception
    {
        for (int i = 0; i < 10; i++) {
            System.out.println("当前时间：" + new Date());
            Thread.sleep(1000);
        }

    }

}
```

输出结果：

    当前时间：Sat Oct 07 21:01:24 CST 2017
    当前时间：Sat Oct 07 21:01:25 CST 2017
    当前时间：Sat Oct 07 21:01:26 CST 2017
    当前时间：Sat Oct 07 21:01:27 CST 2017
    当前时间：Sat Oct 07 21:01:28 CST 2017
    当前时间：Sat Oct 07 21:01:29 CST 2017
    当前时间：Sat Oct 07 21:01:30 CST 2017
    当前时间：Sat Oct 07 21:01:31 CST 2017
    当前时间：Sat Oct 07 21:01:32 CST 2017
    当前时间：Sat Oct 07 21:01:33 CST 2017

###线程让步：yield

yield()方法也是Thread的一个静态方法，它也可以让当前正在执行的线程暂停，但它不会阻塞该线程，它只是将该线程转入就绪状态。yield()只是让当前线程暂停一下，让系统的线程调度器重新调度一次，完全可能的情况是：当某个线程调用了yield()方法暂停之后，线程调度器又将其调度出来重新执行。实际上，当某个线程调用了yield()方法暂停之后，只有优先级与当前线程相同，或者优先级比当前线程更高的处于就绪状态的线程才会获得执行的机会

```java
public class YieldTest extends Thread{
    
    public YieldTest(String name) {
        super(name);
    }
    
    public void run() {
        for (int i = 0; i < 50; i++) {
            System.out.println(getName() + " " + i);
            //当i等于20时，使用yield()方法让当前线程让步
            if(i == 20) {
                Thread.yield();
            }
        }
    }
    
    public static void main(String[] args) {
        //启动两个并发线程
        YieldTest yt1 = new YieldTest("高级");
        //将yt1线程设置成最高优先级
        yt1.setPriority(MAX_PRIORITY);
        yt1.start();
        YieldTest yt2 = new YieldTest("低级");
        //将yt2线程设置成最低优先级
        yt2.setPriority(MIN_PRIORITY);
        yt2.start();
    }
}
```

输出结果：

    ...
    高级 45
    高级 46
    高级 47
    高级 48
    高级 49
    低级 4
    低级 5
    低级 6
    低级 7
    低级 8
    低级 9
    低级 10
    ...

高优先级的线程调用yield()方法暂停之后，系统中没有与之优先级相同，或更高优先级的线程，所以该线程继续执行

sleep()和yield()方法的区别（略）

通常不建议使用yield()方法来控制并发线程的执行

###改变线程优先级

Thread类提供了setPriority(int newPriority)、getPriority()方法来设置和返回指定线程的优先级。其中setPriority()方法的参数可以是一个整数。范围是1~10，也可以使用Thread类的如下三个静态常量

- MAX_PRIORITY，10
- MIN_PRIORITY，1
- NORM_PRIORITY，5

高优先级的线程将会获得更多的执行机会

```java
public class PriorityTest extends Thread{
    
    public PriorityTest(String name) {
        super(name);
    }
    
    public void run() {
        for (int i = 0; i < 50; i++) {
            System.out.println(getName() + ",其优先级是："
                    + getPriority() + ",循环变量的值为：" + i);
        }
    }

    public static void main(String[] args) {
        //改变主线程的优先级
        Thread.currentThread().setPriority(6);
        for (int i = 0; i < 30; i++) {
            if (i == 10) {
                PriorityTest low = new PriorityTest("低级");
                low.start();
                System.out.println("创建之初的优先级："
                        + low.getPriority());
                // 设置该线程为最低优先级
                low.setPriority(Thread.MIN_PRIORITY);
            }
            
            if (i == 20) {
                PriorityTest high = new PriorityTest("高级");
                high.start();
                System.out.println("创建之初的优先级："
                        + high.getPriority());
                // 设置该线程为最低优先级
                high.setPriority(Thread.MAX_PRIORITY);
            }
            
        }

    }

}
```

虽然Java提供了10个优先级级别，但是不同操作系统上的优先级并不相同，而且也不能很好地和Java的10个优先级对应，所以尽量使用MAX_PRIORITY、MIN_PRIORITY和NORM_PRIORITY三个静态常量来设置优先级，这样才可以保证程序具有最好的可移植性

##线程同步





#13.网络编程
IP：给我们提供了唯一的IP地址（其他先不管）

TCP：可靠但是慢

UDP：不可靠但是快

有空看看《TCP/IP详解》

三次握手：

“喂，你听得到吗？”
“我听得到呀，你听得到我吗？”
“我能听到你，今天balabala....”



##Socket
- 两个Java应用程序可通过一个双向的网络通信连接实现数据交换，这个双向链路的一端称为一个Socket。Socket（插座）
- Socket通常用来实现client-server连接
- java.net包中定义的两个类Socket和SeverSocket，分别用来实现双向连接的client和server
- 建立连接是所需的寻址信息为远程计算机的IP地址和端口号（Port number），端口号，两个字节，2^16=65536，最多可以有六万多个应用程序
    - TCP端口 UDP端口分开
    - 每个端口65536
    - 1024之前的端口号不能用，已被操作系统使用





#Java专题————JDBC

数据表：服装 clothes

| 编号 |  名称 | 价格 |
|------|-------|------|
|  001 | 杂牌1 |  100 |
|  002 | 杂牌2 | 50   |

Java中的类和数据表的对应关系

类->表
列->类字段，成员变量
表中每行数据->对象
public class Clothes{
    String number;
    String name;
    double price;
}

new Clothes("001", "杂牌", "价格")

SQL语句

    创建数据库
    CREATE DATABASE mybase;

    使用数据库
    USE mybase;

    创建数据表的格式
    CREATE TABLE 表名(
        列名1 数据类型 约束,
        列名2 数据类型 约束,
        列名3 数据类型 约束
    );





#Java专题————反射机制



#坦克大战
##目的

1. 复习J2SE，综合运用J2SE所学知识
2. 初步掌握面向对象编程的基本思想
3. 掌握Eclipse开发J2SE程序的基本方法
4. 初步掌握Eclipse调试程序的方法（debug）
5. 掌握编程时一些约定俗成的东西
6. 掌握一些常用的编程方法
    - getters, setters
    - 持有对方引用
    - 定义常量
    - 保留程序版本
    - 学会版本比较

最重要的目的是复习J2SE的知识








