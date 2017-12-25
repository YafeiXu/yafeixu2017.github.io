`#  Java学习笔记
## 第四章-类、包和接口
### 4.1-类，字段和方法
+ 字段是类的属性用变量表示，方法是类的动作用函数表示。
+ constructor方法称为构造方法，同类名同名，没有返回数据类型。
```java
Person (String n, int a){\\构造方法的例子
	name = n;
	age = a; 
}
```
+ 默认构造方法不带参数，方法体为空。
+ 调用方法和字段值使用点`p.Sayhalo(); p.name;`。
+ 重载overload主要看参数的个数和参数的类型是否相同。通过重载可以实现多态。
+ 可以使用this访问字段和方法`this.field` 和`this.method()` ，此外this可以调用构造方法，需要放在构造函数中的第一行。

### 4.2-类的继承: 类的继承、字段与方法的增加与覆盖
+ 一个类只能有一个父类superclass。
+ 继承的一个例子
```java
class Student extends Person{
	String school;\\相比于person增加一些字段
	int score;
	boolean isGood(){return score>80};
}
```
+ 任何一个类都是Object的子类。
+ 继承包括字段继承和方法继承（之后可以使用Override修改父类的方法，可以使用注释标记@Override）。
+ 重载本质上是增加新的方法，但是参数数量和参数类型可能不同。
+ 关键字super用于访问父类的字段和方法，比如`int a=super.age;`，调用父类方法`super.sayHalo()`。
+ super调用父类的构造方法。`super(name,age);`super调用构造方法必须放在第一句。
+ 子类与父类的转换
```java
Person p = new Person("Felix",23);
Person s1 = new Student("Andy",33)
Student s2 = (Student) s1;\\左大不用变，左小需要强制类型转换
```

### 4.3-包: 包的含义及使用
+ `package`关键字主要用于导入包。子类和父类可以位于不同包。主要为解决名字空间问题。`package pkg1.pkg2...`
+ 同一个包中各个类可以相互访问。
+ `import`关键字用于导入类。`import java.util.Date` 之后可以直接写`Date dDate =new Date() `否则需要写成`java.util.Date d =new java.util.Date() `

### 4.4-访问控制符: public，protected，private及默认
+ 通常分为两种，访问修饰符（public和private）和其他修饰符即非访问控制符（abstract），这些东西可以用在字段和方法上。
+ 常见的访问控制符（权限修饰符）作用域，字段方法都可以加。
|-----------           | 同一个类 | 同一个包 | 不同包的子类 | 不同包的非子类 |
|-----------|----------|----------|--------------|----------------|
| private   | YES      |          |              |                |
| default什么都不写   | YES      | YES      |              |                |
| protected | YES      | YES      | YES          |                |
| public    | YES      | YES      | YES          | YES            |


+ 对于类class修饰符，只能用public（该类和其他类可以访问）或者默认（只能被同包的类访问）。
+ 字段一般用private修饰，使用set和get方法对字段做存储和取出。

```java
class Person2{
	private int age;\\对类字段使用private
	public int setAge(){\\对类字段设定使用set函数
		return age;
	}
}
```

### 4.5-其他修饰符: static， final， abstract
+非访问控制修饰符主要有三个，static，final，abstract。
| 。       | 基本含义   | 修饰类         | 修饰成员（字段和方法） | 修饰局部变量（方法里面的变量） |
|----------|------------|----------------|----------|--------------|
| static   | 非实例     | 可以修饰内部类 | YES      |              |
| final    | 不可改变   | YES            | YES      | YES          |
| abstract | 不可实例化 | YES            | YES      |              |


+ static字段不属于任何一个实例对象而是属于类，可以通过类名直接访问。`static int Age;`那么Age可以通过两种方法访问`Person.Age` 或者 `p.Age` 都可以。
+ static方法和static字段类似，但是在static方法中不能出现this和super。调用static方法（或者成为类方法）`Math.random()`。
+ final`类则表示不能被继承。final方法表示不能被override。
+ final字段是只读的只能赋值一次。一个字段被static 和 final修饰表示常数。如`Math.PI`。
+ abstract方法`abstract int abstractMethod();`


### 4.6-接口: 接口的定义及实现
+ interface都是**public abstract**的类，接口本质上就是一种特殊的类（是引用类型），主要用于被implements（实现多重继承）。
+ 实现接口使用implements。
+ 三大引用类型，class，interface，array。
```java
interface Collection{
	void add(Object obj);
	void delete(Object obj);
}
class FIFOqueue implements Collection{\\使用implements调用接口类
...
}
Collection c=new FIFOqueue()
```

+ 接口中常量是public static final，一般大写。
+ 一个文件只能有一个public的类，该类名和文件.java名相同。






## 第五章-Java的一些语法注意
### 5.1-变量及其传递
+ 变量分为基本类型primitive和引用类型reference（使用new创建，包括class，interface，array）。
+ 局部变量定义在method中需要显式赋值，字段变量定义在class中不需要显式赋值因为可以自动赋值。
+ 字段变量可以加修饰符public private static final，局部变量只能加final。

### 5.2-多态和虚方法调用
+ overload（编译时多态）是同名method但是通过不同参数调出不同activity。
+ override（运行时多态）是子类对父类的method覆盖。被称为虚方法调用。
+ upcasting的例子`Person p = new Student()`，本质上就是左父右子。虚方法调用时根据实际instance调用method，本例中调用Student的method而不是Person的method。虚方法也可以理解为根据实例instance调用不同method。所有的非final的method都会做虚方法调用，这也叫动态绑定。
+ override主要作用就是在子类继承父类之后可以对父类的方法做出更改。
+ instanceof函数可以用作判断是否是子类。` if(things[i] instanceof Integer){}`
+ static和private的method不是虚方法
+ 三种非虚方法是 static（只跟声明的class有关跟instance无关，即只和等号左边的class有关），private，final。

### 5.3-对象的构造和初始化
+ 构造方法一般通过constructor函数创建，如果没有的话会默认创建。
+ 使用this调用本类构造方法，使用super调用父类构造方法。
+ 构造函数和类名一致，`this()`或者`super()` 放在类定义的第一句。
+ 初始化的一个例子`p=Person(){{age=18;name="Liming";}};` 其中Person是构造函数。
+ instance初始化（使用花括号{}）和static初始化（使用static{}）先与构造函数初始化执行。
+ 构造方法顺序是调用super和this，然后调用static初始化和实例初始化（包括字段赋值），最后调用constructor中其他语句。

### 5.4-对象的清除与垃圾回收
+ 每个对象都有引用计数器。
+ System.gc()是垃圾回收函数，是一个static方法。
+ 在子类中对fianalize()做overide可以释放系统资源。
+ try with resource来清除一些资源，主要是关闭文件如Scanner.close()

### 5.5-内部类与匿名类
+ 简单说内部类就是在class定义的类内部再写一个class，内部类不能与外部类同名。主要作用是可以写一些内部函数与定义一些内部字段。
+ 在其他地方使用内部类时候，要在内部类前面加外部类的名字，创建object时候使用内部类的代码是`outerClassName.new innerClassName()`
```java
class Parcel{//定义一个类包含两个内部类
	class Content{};
	class Destination{};
}
Parcel p = new Parcel(); 
Parcel.Content c = p.new Content(); //在外部创建两个内部类定义的instance
Parcel.Destination d =p.new Destination();
```
+ 内部类访问和外部类同名的字段和方法时候可以使用`outerClassName.this.field` 调用。相对应的外部类调用本身的字段使用`this.field`，任何调用本类花括号中的field都是这么写。类顶层的字段可以直接调用。一个比较的例子，
```java
System.out.println(s);//调用顶层字段
System.out.println(this.s);//调用本类域中的字段
System.out.println(outerClassName.this.s);//调用外部类域中的字段
```
+ 内部类，字段，方法三个都是class的成员。内部类的访问控制修饰字有public，private，protected，默认。还有final（表示不可继承）和abstract。还有static（表示跟对象无关，内部类使用static变为外部类，一般内部类不用static修饰）
+ 外部类只能使用public和默认，外部类**不能**使用private和protected。
+ static类不用使用new创建object。内部类使用static只能访问外部类中的static方法，不能访问外部类中的非static的字段和方法，例子如下
```java
class Parcel{//定义一个类包含两个内部类
	class Content{};
	class static Destination{};
}
Parcel p = new Parcel(); 
Parcel.Content c = p.new Content(); //使用非static类创建object，需要p.new
Parcel.Destination d =new Parcel.Destination(); //使用static类创建object，不需要p.new，因为使用了static的类就相当于一个外部类
```
+ 在一个函数method中定义的类称为局部类local class。局部类不能使用public，private，protected，static但是可以使用final和abstract。可以访问外部类成员，但是不能访问该method的非final的局部变量，因为local variable当method结束后就被回收了，但是final变量类似于常数所以永远存在因此final的可以访问，普通的local variable就不可访问。
+ **匿名类**是一种特殊的内部类，没有类名（使用父类和接口的类名）。匿名类使用new定义，定义后即创建instance，不使用关键字class，extends，implements。不能定义构造方法。匿名类常用场景，如时间监听器，array排序。

### 5.6-Lambda表达式
+ lambda表达式常见语法`(arguments参数)->result函数表达式` ，本质上就是一个函数，作用是把匿名类简写，几个例子，
```java
(String s) -> s.length()
x -> x*x
() -> {System.out.println("gogogo");}
```
+ 把一个普通的实例创建用lambda表达式表示出来，
```java
Runnable dolt = new Runnable(){\\这是一个匿名类创建instance
	public void run(){
		System.out.println("go");
	}
};
new Thread(dolt).start();
```
```java
Runnable dolt = () -> System.out.println("go");\\使用lambda表达式简化
new Thread(dolt).start();
```
```java
new Thread(() -> System.out.println("go")).start();\\更简化的lambda表达式 
```
+ lambda表达式是**接口**和**接口函数**的简写，`interface Fun{double fun(double x);}`
+ lambda函数的几点注意
 + 只能有一个函数。
 + 可以用`@FunctionalInterface`来标记。 称为函数式接口，可写也可以不写。
```java
Arrays.sort(people, (p1,p2)->(int)(p1.score-p2.score));\\排序例子
```

### 5.7-装箱、枚举、注解(*较高要求)
+ 基本类型的包装类，其本质是一个类定义，如int对应的Integer一共八类，Boolean, Byte, Short,  Character, Integer, Long, Float, Double
```java
Integer I = new Integer(10);\\把I变成引用类型
Integer I = Integer.valueOf(10);\\装箱
Integer I = 10;
int i = I.intValue();\\拆箱
int i = I;
```
+ **枚举**是一种引用类型，本质上是一个类。
```java
enum Light{red, yellow, green};\\定义一个枚举类
Light light=Light.red;\\使用枚举类Light生成一个light变量
```
+ **注解**本质上也是一个类。可以自定义注解。
```java
@Override
@Deprecated
@SuppressWarnings
```

### 5.8-没有指针的Java语言:引用与指针(*较高要求)
+ 引用类型相当于指针。指针运算可以用数组代替。函数指针可以用lambda表达式与接口函数。
+ **相等判断问题==**，基本类型是值相等，引用类型是应用相等。浮点类型要注意。`Integer m=10; Integer n=10; m==n\\ true 因为有缓存-128到+127都没问题，虽然Integer生成的m和n是两个instance。`
+ 枚举类型可以判断。
+ 对于引用对象一般都要使用equals方法或hashCode方法判断。比如字符串String判断使用equals方法判断是否相等。但是对于字符串常量那么可以使用==判断。
```java
String a="halo", b="halo";
System.out.println(a==b);\\ true
```


## 第六章-异常处理

### 6.1-异常处理: 异常的抛出与捕获
+ 基本语法
```java
try{
... ;
}catch(Exception ex){
... ;
}
```
+ Java两种异常runtimeException和checked Exception（比如IO异常）。受检异常必须处理，要么catch要么throw。

### 6.2-自定义异常

### 6.3 断言及程序的测试
+ assert 逻辑表达式 : 异常的提示信息（not true），用于程序检测和调试。
```java
class testCode{
	public static void main(String[] args){
		assert TriArea(3,4)==5 : "wrong Answer"
	}
	static double TriArea(double a, double b){
		return Math.sqrt(a*a+b*b);
	}	
}
```

+ 使用JUnit测试程序。常用assertEquals判断是否通过测试。

### 6.4 程序的调试 debug
+ 程序常见错误，语法错误（IDE处理），运行错误（try，catch处理），逻辑错误（debug，JUnit单元测试）。
+ debug三种方法，断点（右键toggle point），跟踪（F5），监视（鼠标指向变量，右键调出监视窗口）。


## 第七章-工具类及常用算法
### 7.1 Java语言基础类:Object、Math、System类
+ Object类是所有类的父类。
+ ==判断引用是否相等，equals判断内容是否相等。
```java
Integer one = new Integer(1);
Integer anotherOne = new Integer(1);
one==anotherOne; \\ false
one.equals(anotherOne);\\ true
```
+ obj.getClass()表出类。
	+ obj.toString()返回对象字符串表示。valueOf(String)。
+ Java中存在基本数据类型的包装类，把基本数据类型编程引用类型。
+ `Integer I=5` 装包`int i=I` 拆包。
+ Math类有很多static方法。





