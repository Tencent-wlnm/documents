[toc]

# JavaSE细节

## 1. 了解Java

### 1.1 Java历史

1. 1995年sun正式发布java第一个版本
2. 2009年，甲骨文公司（Oracle）收购SUN公司
3. Java之父是詹姆斯·高斯林 （James Gosling）
4. 目前企业用的最多的JDK版本是java8和java11
5. java8和java11都是LTS版本（Long-Term-Support）

### 1.2 Java技术体系平台

1. Java SE(Java Standard Edition)标准版
2. Java EE(Java Enterprise Edition)企业版
3. Java ME(Java Micro Edition)小型版

### 1.3 Java特点

1. 面向对象（OOP）
2. 健壮（强类型机制、异常处理、垃圾自动回收）
3. 跨平台
4. 解释型语言（编译后的代码需要解释器来执行）

### 1.4 JDK、JRE、JVM的包含关系

1. **JDK = JRE +** **开发工具集**（例如 Javac,java 编译工具等）
2. **JRE = JVM + Java SE** **标准类库**（java 核心类库）

### 1.5 Java API文档

> API——Application Programming Interface,应用程序编程接口
>
> [中文在线文档](https://www.matools.com)

## 2. 数据类型

### 2.1 Java的引用数据类型包括

1. 类class
2. 接口interface
3. 数组

### 2.2 关于浮点数的说明

- 浮点数=符号位+指数位+尾数位
- 尾数部分可能丢失，造成精度损失（小数都是近似值）
- double型浮点字面量可以在末尾加d/D
- 小数点前可以为空，如：.123=0.123
- 可以用科学计数法表示，如：5.12e-2=0.0512
- 使用诸如Math.abs(num1-num2)<1E-7来判定是否相等

### 2.3 自动类型转换

1. char->int->long->float->double
2. byte->short->int->long->float->double
3. 混合运算时先自动转换为容量最大的类型
4. byte/short和char之间不会互相自动转换
5. byte,short,char只要参与运算就会升级为int
6. boolean不参与转换

```java
//自动类型转换细节 
public class AutoConvertDetail { 
    
    public static void main(String[] args) { 
        //细节 1： 有多种类型的数据混合运算时， 
        //系统首先自动将所有数据转换成容量最大的那种数据类型，然后再进行计算 
        int n1 = 10; //ok 
        float d1 = n1 + 1.1;//错误 n1 + 1.1 => 结果类型是 double
        double d1 = n1 + 1.1;//对 n1 + 1.1 => 结果类型是 double 
        float d1 = n1 + 1.1F;//对 n1 + 1.1 => 结果类型是 float 
        
        //细节 2: 当我们把精度(容量)大 的数据类型赋值给精度(容量)小 的数据类型时， 
        //就会报错，反之就会进行自动类型转换。
        int n2 = 1.1;//错误 double -> int 
        //细节 3: (byte, short) 和 char 之间不会相互自动转换 
        //当把具体数赋给 byte 时，(1)先判断该数是否在 byte 范围内，如果是就可以 
        byte b1 = 10; //对 , -128-127 
        int n2 = 1; //n2 是 int 
        byte b2 = n2; //错误，原因： 如果是变量赋值，判断类型 //
        char c1 = b1; //错误， 原因 byte 不能自动转成 char //
        //细节 4: byte，short，char 他们三者可以计算，在计算时首先转换为 int 类型
        byte b2 = 1;
        byte b3 = 2;
        short s1 = 1; 
        short s2 = b2 + s1;//错, b2 + s1 => int 
        int s2 = b2 + s1;//对, b2 + s1 => int
        byte b4 = b2 + b3; //错误: b2 + b3 => int 
        //boolean 不参与转换 
        boolean pass = true; 
        //int num100 = pass;// boolean 不参与类型的自动转换 
        //自动提升原则： 表达式结果的类型自动提升为 操作数中最大的类型 
        //看一道题 
        byte b4 = 1;
        short s3 = 100; 
        int num200 = 1; 
        float num300 = 1.1F; 
        double num500 = b4 + s3 + num200 + num300; //float -> double 
    }
}
```

### 1.9 基本数据类型与String类型的转换

```java
public class StringToBasic{
    public static void main(String[] args){
        int a = 1;
        float b = 1.1F;
        boolean c = true;
        String s1 = a + "";
        String s2 = b + "";
        String s3 = c + "";
        
        int aa = Integer.parseInt("111");
        double bb = Double.parseDouble("1.123");
        float cc = Float.parseFloat("123.45");
        short dd = Short.parseShort("12");
        byte ee = Byte.parseByte("127");
        boolean ff = Boolean.parseBoolean("true");
        long gg = Long.parseLong("1232134124123");
        char tt = "abcd".charAt(0);//tt=a
    }
}
```

## 3. 运算符

### 3.1 算术运算符

- 取模公式：==a%b=a-(int)a/b*b==

```java
public class ArithmeticOperator { 
	public static void main(String[] args) {
		System.out.println(10 / 4); //从数学来看是2.5, java中 2
		System.out.println(10.0 / 4); //java是2.5

		double d = 10 / 4;//java中10 / 4 = 2, 2=>2.0
		System.out.println(d);// 是2.0

		// % 取模 ,取余
		// 在 % 的本质 看一个公式!!!! a % b = a - a / b * b
		// -10 % 3 => -10 - (-10) / 3 * 3 = -10 + 9 = -1
		// 10 % -3 = 10 - 10 / (-3) * (-3) = 10 - 9 = 1
		// -10 % -3 =  (-10) - (-10) / (-3) * (-3) = -10 + 9 = -1
		System.out.println(10 % 3); //1

		System.out.println(-10 % 3); // -1
		System.out.println(10 % -3); //1
		System.out.println(-10 % -3);//-1
        
        //当a为小数时
        //-10.5%3=-10.5-(int)(-10.5)/3*3=-10.5-(-9)=-1.5
        System.out.println(-10.5%3);//-1.5
        
		// int i = 1;//i->1
		// i = i++; //规则使用临时变量: (1) temp=i;(2) i=i+1;(3)i=temp;
		// System.out.println(i); // 1
		
		// int i=1;
		// i=++i; //规则使用临时变量: (1) i=i+1;(2) temp=i;(3)i=temp;
		// System.out.println(i); //2
        
		int i1 = 10;
	    int i2 = 20;
        int i = i1++;
        System.out.print("i="+i);//10
        System.out.println("i2="+i2);//20
        i = --i2; 
        System.out.print("i="+i);//19
        System.out.println("i2="+i2);//19
	}
}
```

### 3.2 赋值运算符

- 复合赋值运算符会进行类型转换

```java
byte b = 3; 
b += 2; // 等价 b = (byte)(b + 2); 
b++; // b = (byte)(b+1);
```

## 4. 数组

### 4.1 数组有默认值

> int 0，short 0, byte 0, long 0, float 0.0,double 0.0，char \u0000，boolean false，String null...

### 4.2 数组初始化的方式

1. 一维数组

```java
//动态初始化
//1.先声明
int a[];
int[] aa;
//2.再创建
a = new int[10];
//数组长度不要求是常量
//new int[n];会创建一个长度为n的数组
//一旦创建好，就不能更改大小
//可以使用Arrays.copyOf来增加数组的大小

//一气呵成
int b = new int[10];

//静态初始化
int c = {1, 2, 3, 4};

//匿名数组
new int[]{1,2,3,4};
//使用这种语法可以在不创建新变量的情况下重新初始化一个数组
int[] abc = {1,2,3};
abc = new int[]{4,5,6};

//数组长度可以为0，在编写一个返回数组的方法时，如果结果为空，可以创建一个长度为0的数组
new elementType[0];
//数组长度为0与null不同
```

2. 二维数组

```java
//动态初始化
//先声明
int a[][];
int[] aa[];
int[][] aaa;
//再创建
a = new int[2][3];

//一气呵成
int[][] a = new int[2][3];

//列数不确定
//创建一个二维数组，有3个元素都是一维数组，但是每个一维数组还没有开数据空间
int[][] arr = new int[3][]; 

for(int i = 0; i < arr.length; i++) {//遍历arr每个一维数组
    //给每个一维数组开空间 new
    //如果没有给一维数组 new ,那么 arr[i]就是null
    arr[i] = new int[i + 1]; 

    //遍历一维数组，并给一维数组的每个元素赋值
    for(int j = 0;  j < arr[i].length; j++) {
        arr[i][j] = i + 1;//赋值
    }
}
//遍历arr输出
for(int i = 0; i < arr.length; i++) {
    //输出arr的每个一维数组
    for(int j = 0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
    }
    System.out.println();//换行
}


//静态初始化
int[][] b = {{1,2,3},{1,2},{1}};
```

## 5. 面向对象编程

### 5.1 面向对象基础

#### 5.1.1 实例变量（属性）有默认值

> int 0，short 0, byte 0, long 0, float 0.0,double 0.0，char \u0000，boolean false，String null...

#### 5.1.2 创建对象的流程（面试题）

```java
class A{
    int a = 1;
    double d;
    public A(int a,double d){
        this.a = a;
        this.d = d;
    }
}
class B{
    public static void main(String[] args){
        A a = new A(2,3.14);
    }
}
```

1. 加载类（A.class）的信息（只加载一次）
2. 在堆中开辟一块空间
3. 完成对象初始化（默认初始化a=0,d=0.0;显式初始化a=1;构造器初始化a=2,d=3.14）
4. 将对象在堆中的内存地址返回给对象的引用

#### 5.1.3 方法重载

1. 方法名必须相同
2. 形参列表必须不同(形参类型、个数、顺序，至少有一样不同，形参名没要求)
3. 返回值类型无要求

#### 5.1.4 可变参数

```java
public class VarParameter01 { 
	//编写一个main方法
	public static void main(String[] args) {
		HspMethod m = new HspMethod();
		System.out.println(m.sum(1, 5, 100)); //106
		System.out.println(m.sum(1,19)); //20
	}
}

class HspMethod {
	//可以计算 2个数的和，3个数的和 ， 4. 5， 。。
	//可以使用方法重载
	// public int sum(int n1, int n2) {//2个数的和
	// 	return n1 + n2;
	// }
	// public int sum(int n1, int n2, int n3) {//3个数的和
	// 	return n1 + n2 + n3;
	// }
	// public int sum(int n1, int n2, int n3, int n4) {//4个数的和
	// 	return n1 + n2 + n3 + n4;
	// }
	//.....
	//上面的三个方法名称相同，功能相同, 参数个数不同-> 使用可变参数优化
	//老韩解读
	//1. int... 表示接受的是可变参数，类型是int ,即可以接收多个int(0-多) 
	//2. 使用可变参数时，可以当做数组来使用 即 nums 可以当做数组
	//3. 遍历 nums 求和即可
	public int sum(int... nums) {
		//System.out.println("接收的参数个数=" + nums.length);
		int res = 0;
		for(int i = 0; i < nums.length; i++) {
			res += nums[i];
		}
		return res;
	}
}
```

1. 可变参数的实参可以为==0个或任意多个==
2. 可变参数的实参可以为数组
3. 可变参数的本质就是数组
4. 可变参数可以和普通类型的参数一起放在形参列表，但必须放在形参列表最后
5. 一个形参列表最多有一个可变参数

```java
public class VarParameterDetail { 
	//编写一个main方法
	public static void main(String[] args) {
		//细节: 可变参数的实参可以为数组
		int[] arr = {1, 2, 3};
		T t1 = new T();
		t1.f1(arr);
	}
}

class T {
	public void f1(int... nums) {
		System.out.println("长度=" + nums.length);
	}
	//细节: 可变参数可以和普通类型的参数一起放在形参列表，但必须保证可变参数在最后
	public void f2(String str, double... nums) {
	}
	//细节: 一个形参列表中只能出现一个可变参数
	//下面的写法是错的.
	// public void f3(int... nums1, double... nums2) {
	// }
}
```

#### 5.1.5 成员变量与局部变量

1. 成员变量有初始值，可以直接使用；局部变量没有初始值，必须赋值后才能使用。
2. 全局变量可以加发访问修饰符，局部变量不行。

#### 5.1.6 构造器

1. 构造器的访问修饰符可以为public、protected、默认、private
2. 在创建对象时，系统会自动的调用该类的构造器完成对象的初始化
3. 构造器也可以重载
4. ==构造器是用来完成对象的初始化的，并不是创建对象==

#### 5.1.7 this关键字

1. this 关键字可以用来访问本类的成员变量、方法、构造器 
2. this 用于区分当前类的属性和局部变量
3. 访问成员方法的语法：this.方法名(参数列表);
4. 访问构造器语法：this(参数列表); 只能在一个构造器中访问另外一个构造器, 且==必须放在第一条语句==
5. this不能在类定义的外部使用，只能在类定义的内部使用(类方法外也可)

```java
public class Test {
    int a = 1;
    int b = this.a;
}
```

### 5.2 面向对象中级

#### 5.2.1 包

1. 包的命名规则同标识符命名规则
2. 包的命名一般为com.公司名.项目名.业务模块名

一个包下,包含很多的类,java 中常用的包有: 

1. java.lang.* //lang 包是基本包，默认引入，不需要再引入. 
2.  java.util.* //util 包，系统提供的工具包, 工具类，使用 Scanner 
3. java.net.* //网络包，网络开发 
4. java.awt.* //是做 java 的界面开发，GUI

#### 5.2.2 访问修饰符

| 访问级别 | 访问控制修饰符 | 本类 | 同包 | 子类 | 不同包 |
| :------: | :------------: | :--: | :--: | :--: | :----: |
|   公开   |     public     |  O   |  O   |  O   |   O    |
|  受保护  |   protected    |  O   |  O   |  O   |   X    |
|   默认   |       无       |  O   |  O   |  X   |   X    |
|   私有   |    private     |  O   |  X   |  X   |   X    |

1. 访问修饰符可以用来修饰成员变量、成员方法、类、接口、注解
2. 类只能用默认和public修饰

#### 5.2.3 构造器和set方法

直接在构造器中对成员变量初始化可能会引发数据异常，应该在构造器中调用写好的set方法，这样在初始化成员变量时也能检测数据是否合理。

```java
public Person(String name, int age, double salary) { 
    // this.name = name; 
    // this.age = age; 
    // this.salary = salary; 
    //我们可以将 set 方法写在构造器中，这样仍然可以验证 
    setName(name); 
    setAge(age); 
    setSalary(salary); 
}
```

#### 5.2.4 继承

1. ==子类继承了父类所有的成员变量和成员方法（包括私有和默认的）==，成员变量和成员方法在子类访问时需要遵守访问控制修饰符, 私有的成员不能在子类直接访问（子类和父类不在同一个包下时，默认的也不能在子类中直接访问），要通过父类提供公共方法来访问
2. 子类必须调用父类的构造器，完成父类的初始化
3. 当创建子类对象时，不管使用子类的哪个构造器，默认情况下总会去调用父类的无参构造器，如果父类没有提供无参构造器，则必须在子类的构造器中用 super 去指定使用父类的哪个构造器完成对父类的初始化工作，否则，编译不会通过
4. 如果希望指定去调用父类的某个构造器，则显式的调用一下 : super(参数列表) 
5. super() 和 this() 都只能在构造器中使用，且必须放在构造器第一行，因此这两个方法不能共存在同一个构造器中
6. java 所有类都是 Object 类的子类, Object 是所有类的父类. 
7. 父类构造器的调用不限于直接父类！将一直往上追溯直到 Object 类(顶级父类) 
8. 子类最多只能继承一个父类(指直接继承)，即 java 中是单继承机制。
9. 不能滥用继承，子类和父类之间必须满足 is-a 的逻辑关系
10. IDEA中ctrl+H可以看到类之间的继承关系

```java
/**
 * 继承的本质
 */
public class ExtendsTheory {
    public static void main(String[] args) {
        Son son = new Son();
        //要按照查找关系来返回信息
        //(1) 首先看子类是否有该属性
        //(2) 如果子类有这个属性，并且可以访问，则返回信息
        //(3) 如果子类没有这个属性，就看父类有没有这个属性(如果父类有该属性，并且可以访问，就返回信息..)
        //(4) 如果父类没有就按照(3)的规则，继续找上级父类，直到Object...
        System.out.println(son.name);//返回就是大头儿子
        //System.out.println(son.age);//返回的就是39
        //System.out.println(son.getAge());//返回的就是39
        System.out.println(son.hobby);//返回的就是旅游
    }
}

class GrandPa { //爷类
    String name = "大头爷爷";
    String hobby = "旅游";
}

class Father extends GrandPa {//父类
    String name = "大头爸爸";
    private int age = 39;

    public int getAge() {
        return age;
    }
}

class Son extends Father { //子类
    String name = "大头儿子";
}
```

#### 5.2.5 super关键字

> 在子类中访问父类的成员方法、成员变量、构造器，用于区分子类与父类的同名成员（变量/方法）

1. 使用super关键字访问父类成员时同样要遵守访问控制修饰符的相关规则
2. 当子类中有成员与父类成员重名时，要想访问父类成员，必须使用super；没重名时使用super,this或直接访问父类成员的效果一样
3. super的访问不限于直接父类，如果爷爷类有与本类相同的成员，也可以用super访问爷爷类的成员
4. 如果多个上级类都有相同的成员，遵循就近原则
5. 在本类中访问属性或方法时，先从本类找，如果有就访问成功；如果没有就去父类找，在父类中找到了就看访问权限，权限足够，访问成功，不够就会报错cannot access;父类没有就找父类的父类，直到Object类，最终还找不到就会提示成员不存在。
6. 如果在本类中使用super访问成员，就会跳过本类，直接从父类开始找。

```java
public class B extends A {
    
    public void cal() {
        System.out.println("B类的cal() 方法...");
    }
    public void sum() {
        System.out.println("B类的sum()");
        //希望调用父类-A 的cal方法
        //这时，因为子类B没有cal方法，因此我可以使用下面三种方式

        //找cal方法时(cal() 和 this.cal())，顺序是:
        // (1)先找本类，如果有，则调用
        // (2)如果没有，则找父类(如果有，并可以调用，则调用)
        // (3)如果父类没有，则继续找父类的父类,整个规则，就是一样的,直到 Object类
        // 提示：如果查找方法的过程中，找到了，但是不能访问， 则报错, cannot access
        //      如果查找方法的过程中，没有找到，则提示方法不存在
        //cal();
        this.cal(); //等价 cal

        //找cal方法(super.call()) 的顺序是直接查找父类，其他的规则一样
        //super.cal();

        //演示访问属性的规则
        //n1 和 this.n1 查找的规则是
        //(1) 先找本类，如果有，则调用
        //(2) 如果没有，则找父类(如果有，并可以调用，则调用)
        //(3) 如果父类没有，则继续找父类的父类,整个规则，就是一样的,直到 Object类
        // 提示：如果查找属性的过程中，找到了，但是不能访问， 则报错, cannot access
        //      如果查找属性的过程中，没有找到，则提示属性不存在
        System.out.println(n1);
        System.out.println(this.n1);

        //找n1 (super.n1) 的顺序是直接查找父类属性，其他的规则一样
        System.out.println(super.n1);

    }
    //访问父类的方法，不能访问父类的private方法 super.方法名(参数列表);
    public void ok() {
        super.test100();
        super.test200();
        super.test300();
        //super.test400();//不能访问父类private方法
    }
    //访问父类的构造器：super(参数列表);只能放在构造器的第一句，只能出现一句！
    public  B() {
        //super();
        //super("jack", 10);
        super("jack");
    }
}

```

| 区别              | this                                               | super                                  |
| ----------------- | -------------------------------------------------- | -------------------------------------- |
| 访问成员变量/方法 | 访问本类中的成员，没有则从父类继续查找             | 从父类开始查找成员                     |
| 调用构造器        | 在本类的构造器中调用本类的其他构造器，必须放在首行 | 在子类中调用父类的构造器，必须放在首行 |
| 其他              | 表示当前对象                                       | 在子类中访问父类成员                   |

#### 5.2.6 方法覆盖/重写（override）

> 子类中的某个方法与父类中某个方法的方法名、返回类型、形参列表相同，就称子类的这个方法重写了父类的方法

1. 子类方法的返回类型与父类一样或者是父类方法返回类型的子类，比如父类返回类型是Object，子类方法的返回类型可以是String
2. 子类方法不能缩小父类方法的访问权限，比如父类方法的访问权限是protected，子类方法的访问权限就不能是默认或者private，但可以是public
3. 子类方法的方法名和形参列表要和父类的完全一样

方法重载和方法重写/覆盖的对比

| 名称     | 发生范围       | 方法名   | 返回类型                                             | 形参列表                             | 访问修饰符                           |
| -------- | -------------- | -------- | ---------------------------------------------------- | ------------------------------------ | ------------------------------------ |
| 方法重载 | 同一个类       | 必须一样 | 无要求                                               | 形参的类型、个数、顺序至少有一个不同 | 无要求                               |
| 方法重写 | 父类与子类之间 | 必须一样 | 子类的返回类型与父类的一样或者是父类的返回类型的子类 | 必须一样                             | 子类方法的访问权限不能比父类方法的低 |

#### 5.2.7 多态

1. 方法的多态

方法重载和方法重写就体现出多态，即同一个方法传入的参数不同就会调用不同的此方法，不同对象调用同名方法也会调用不同的此方法

2. ==对象的多态==

- 一个对象的编译类型和运行类型是可以不一致的
- 编译类型在定义对象时就确定了，不可改变；运行类型可以改变。
- 编译类型看=的左边，运行类型看=的右边

```java
Animal animal = new Dog();//animal的编译类型为Animal,运行类型为Dog
animal = new Cat();//animal的编译类型仍然为Animal,运行类型为Cat
```

3. 多态的注意事项与细节

- 多态的前提：两个类存在继承关系
- 多态的向上转型

```java
//本质：父类的引用指向了子类的对象
/*特点： 1.可以调用父类的所有成员（需要遵守访问权限）；
		2.不能调用子类中的特有成员
        3. 最终运行效果看子类的具体实现
*/
Animal a = new Cat();
```

- 多态的向下转型

```java
//要求父类的引用必须指向的是当前目标类型的对象
//向下转型后就可以调用子类中的所有成员
Cat c = (Cat)a;
```

4. instanceof

> 比较操作符，用于判断对象的运行类型是否是此类型或此类型的子类型

5. ==java的动态绑定机制==

```java
public class DynamicBinding {
    public static void main(String[] args) {
        //a 的编译类型 A, 运行类型 B
        A a = new B();//向上转型
        System.out.println(a.sum());//?40 -> 30
        System.out.println(a.sum1());//?30-> 20
    }
}

class A {//父类
    public int i = 10;
    //动态绑定机制:

    public int sum() {//父类sum()
        return getI() + 10;//20 + 10
    }

    public int sum1() {//父类sum1()
        return i + 10;//10 + 10
    }

    public int getI() {//父类getI
        return i;
    }
}

class B extends A {//子类
    public int i = 20;

//    public int sum() {
//        return i + 20;
//    }

    public int getI() {//子类getI()
        return i;
    }

//    public int sum1() {
//        return i + 10;
//    }
}

```

5. 多态的应用

- 多态数组：数组的定义类型为父类型，元素的实际类型为子类型
- 多态参数：方法的形参类型为父类型，实参类型为子类型

### 5.3 Object类

#### 5.3.1 equals方法

1. 与==运算符的对比

> 1. 既可以判断基本数据类型，也可以判断引用数据类型
> 2. 如果是基本数据类型，判断的是值是否相等
> 3. 如果是引用数据类型，判断的是地址是否相等（即是否是同一个对象）
> 4. equals方法只能判断引用数据类型，默认判断地址是否相等

```java
//看看Object类的 equals 是
//即Object 的equals 方法默认就是比较对象地址是否相同
//也就是判断两个对象是不是同一个对象.
public boolean equals(Object obj) {
	return (this == obj);
}      
//从源码可以看到 Integer 也重写了Object的equals方法,
//变成了判断两个值是否相同
public boolean equals(Object obj) {
	if (obj instanceof Integer) {
		return value == ((Integer)obj).intValue();
	}
	return false;
}
```

#### 5.3.2 hashCode方法

> 返回对象的哈希码值

1. 提高具有哈希结构的容器的效率
2. 如果两个引用指向的是同一个对象，则哈希值一样
3. 如果两个引用指向的是不同对象，则哈希值是不一样的
4. 哈希值主要是根据内存地址来的，不能完全将哈希值等价于地址

#### 5.3.3 toString()方法

1. 默认返回：全类名+@+哈希值的十六进制。
2. 子类往往重写toString方法，打印对象或拼接对象时，都会自动调用该对象的toString方法
3. 当直接输出对象时，toString方法会被默认的调用

#### 5.3.4 finalize()方法

1. 当对象被回收时，系统自动调用该对象的finalize方法。子类可以重写该方法做一些释放资源的操作
2. 什么时候回收：当某个对象没有任何引用时，则JVM就认为这个对象是一个垃圾对象，就会使用垃圾回收机制来销毁对象
3. 垃圾回收机制的调用是由系统来决定的（CG算法），也可以通过System.gc()建议系统调用垃圾回收器（不一定会调用）

### 5.4 面向对象高级

#### 5.4.1 类变量

> 类变量也叫静态变量，是该类所有对象共享的变量，任何一个该类的对象去访问它时，取到的都是相同的值；任何一个该类的对象去修改它时，修改的也是同一个变量。

##### 如何访问类变量

1. ==类名.类变量名==（类变量随着类的加载而创建，所以即使没有创建对象实例也能访问）
2. 对象名.类变量名（不推荐）
3. 静态变量的访问修饰符的访问权限和范围与实例变量是一样的

##### 使用细节

1. 当需要让某个类的所有对象都共享一个变量时，应该用静态变量
2. 类变量时该类所有对象共享的，而实例变量是每个对象独享的
3. 类变量在类加载时就初始化了，只要类加载，就能访问类变量了
4. 生命周期：随着类的加载开始，随着类的消亡结束

#### 5.4.2 类方法

> 即静态方法
>
> 当方法中不涉及任何对象相关的成员时，可以将方法设计成静态方法

##### 使用细节

1. 静态方法和实例方法都随着类的加载而加载，将结构信息存储在方法区
2. 静态方法中无this参数，实例方法中隐含着this参数
3. 静态方法可以通过类名或对象名调用，实例方法只能用对象名调用
4. 类方法中不允许使用和对象有关的关键字，比如this,super
5. 静态方法只能直接访问静态成员，访问非静态成员需要创建对象
6. 非静态方法可以访问静态成员和非静态成员（遵守访问权限）

#### 5.4.3 代码块

> 又称初始化块，属于类中的成员（即：是类的一部分）

##### 实例代码块

1. 相当于另一种形式的构造器（对构造器的补充机制），可以做初始化操作
2. 如果多个构造器中都有重复的语句，可以抽取到代码块中，提高代码的复用性
3. 每创建一个对象就会执行一次

##### 静态代码块

1. 对类进行初始化
2. 随着类的加载而执行，类只加载一次，所以静态代码块只执行一次

##### 类什么时候加载

1. 创建对象实例时（new）
2. 创建子类对象实例时，父类也会被加载
3. 使用类的静态成员时（静态变量、静态方法）

##### 创建对象时，在一个类的调用顺序

1. 调用静态代码块和静态属性初始化（两者优先级相同）
2. 调用实例代码块和实例变量初始化（两者优先级相同）
3. 调用构造方法

##### 构造器的隐含内容

构造器的执行语句前面其实隐含了super()和调用实例代码块

```java
class A{
    public A{
        //1.super();
        //2.调用实例代码块
        System.out.println("a");
    }
}
```

##### 创建子类对象时的调用顺序

1. 父类的静态代码块和静态变量（优先级一样，按定义顺序执行）
2. 子类的静态代码块和静态变量（优先级一样，按定义顺序执行）
3. 父类的实例代码块和实例变量（优先级一样，按定义顺序执行）
4. 父类的构造方法
5. 子类的实例代码块和实例变量（优先级一样，按定义顺序执行）
6. 子类的构造方法

##### 静态代码块只能直接调用静态成员，实例代码块可以直接调用静态成员和非静态成员

#### 5.4.4 单例设计模式

##### 什么是设计模式

1. 静态方法和实例变量的经典应用
2. 在大量时间中总结和理论化之后优选的代码结构、编程风格以及解决问题的思考方式。

##### 什么是单例设计模式

1. 采取一定的方法保证在整个软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法
2. 单例设计模式有两种：饿汉式、懒汉式
3. java.lang.Runtime就是经典的单例设计模式

##### 单例设计模式的具体实现

1. 构造器私有化（防止直接创建对象）
2. 类的内部创建对象
3. 向外暴露一个静态的公共方法：getInstance

```java
package com.stu_single_instance;

/**
 * @author Roozen
 * @version 1.0
 * 饿汉式单例设计模式
 */
public class Hungry {
    //在类的内部创建对象
    private static Hungry hungry = new Hungry();

    //构造器私有化
    private Hungry() {
        
    }
    
    //向外提供一个返回对象实例的方法
    public static Hungry getInstance() {
        //在这里可以做一些权限设置，比如符合某些条件才返回实例
        return hungry;
    }
}
```

```java
package com.stu_single_instance;

/**
 * @author Roozen
 * @version 1.0
 * 懒汉式单例设计模式
 */
public class Lazy {
    //在类的内部创建对象(默认为null)
    private static Lazy lazy;

    //构造器私有化
    private Lazy() {

    }

    //向外提供一个返回对象实例的方法
    public static Lazy getInstance() {
        //在这里可以做一些权限设置，比如符合某些条件才返回实例

        //如果还没有创建对象就创建对象，否则直接返回对象实例
        if (lazy == null) {
            lazy = new Lazy();
        }
        return lazy;
    }
}
```

##### 饿汉式VS懒汉式

1. 创建对象的时机不同：饿汉式在类加载时创建，懒汉式在需要使用时创建；
2. 饿汉式不存在线程安全问题，懒汉式存在；
3. 饿汉式创建的对象若没有使用就会造成资源浪费，懒汉式不存在此问题；

#### 5.4.5 final关键字

1. final修饰的类不能被继承
2. final修饰的方法不能被重写
3. final修饰的变量是常量

##### 使用细节

1. final修饰的成员变量在定义时必须赋初值（成员变量有默认初始值，所以必须手动赋初值，局部变量定义时可以不赋初值，但只能手动赋值一次，第二次就报错）
2. final修饰的成员变量可以在定义时、构造器中、代码块中赋初值（如果是静态变量，则不能在构造器中赋初值）
3. final类虽然不能继承，但可以实例化对象
4. 如果类不是final类，但有final方法，则该方法不能被重写，但能被继承。
5. 在final类中没必要将方法修饰成final（final类不能被继承，则方法不可能被重写）
6. final不能用来修饰构造器（构造器不能被继承）
7. final和static往往搭配使用，效率高，不会导致类加载（编译器做了优化）
8. 包装类（Integer/Double/Float/Boolean),String类都是final类

#### 5.4.6 抽象类（abstract）

> 当父类的一些方法不能确定如何实现时，可以做成抽象类。抽象类的价值更多在于设计，让子类继承并实现抽象类。在框架和设计模式使用较多。

##### 使用细节

1. 抽象类不能被实例化
2. 抽象类可以不包含抽象方法
3. 有抽象方法的类一定要声明为抽象类
4. abstract只能用来修饰类和方法
5. 抽象类可以有任意成员（本质还是类）
6. 抽象方法不能有方法体
7. 非抽象类继承抽象类必须实现抽象类的所有抽象方法
8. 抽象方法不能使用private/final/static类修饰，这些关键字都是与重写相违背的！！！

```java
//以下全错
//final类不能被继承
abstract final class A{}

//static关键字与方法重写无关
public abstract static void a();

//final方法不能被重写
public abstract final void a();

//private方法不能被重写
private abstract void a();
```

##### 抽象类最佳实践——模板设计模式

> 抽象类体现的就是一种模板模式的设计，抽象类作为多个子类的通用模板，子类在抽象类的基础上进行扩展、改造，但子类总体上会保留抽象类的行为方式。
>
> 能解决的问题：
>
> 1. 当功能内部有部分实现是不确定的，就可以把不确定的部分让子类去实现
> 2. 编写一个抽象父类，提供多个通用方法，把其他方法留给子类实现就是一种模板模式

#### 5.4.7 接口

> 接口就是给出一些没有实现的方法，封装到一起。某个类需要使用时再根据具体情况具体实现。 
>
> 接口是更加抽象的类，抽象类中的方法可以有方法体，接口中所有方法都没有方法体。接口体现了程序设计的多态和高内聚低耦合的设计思想。JDK8.0后接口中可以有静态方法和默认方法，也就是说可以有方法的具体实现。

##### 细节

1. 接口不能实例化
2. 接口中的方法是默认public修饰的，接口中的抽象方法可以不用abstrcat修饰
3. 普通类必须实现接口中所有抽象方法
4. 抽象类实现接口可以不用实现接口的方法
5. 接口中的成员变量是常量，如int a=1;实际上是public static final int a = 1;(必须初始化)
6. 接口可以继承（exteds）多个其他接口，如interface A extends B,C{}
7. 接口的访问修饰符只能是public和默认
8. 接口在一定程度上实现代码解耦合（接口规范性+动态绑定机制）
9. 接口存在==多态传递现象==（即：如果B实现了A接口,C继承了B，那么C也相当于实现了A，可以A类型的引用接收C对象）
10. 接口也与多态数组
11. 接口也可以作为多态参数

#### 5.4.8 内部类

> 一个类的内部嵌套了另一个类结构。
>
> 类的五大成员:成员变量、成员方法、构造器、代码块、内部类
>
> 内部类的最大特点就是可以直接访问外部类的私有属性，并且可以直接体现类之间的包含关系

##### 内部类的分类

1. 定义在局部位置（方法块、代码块中）：局部内部类、匿名内部类
2. 定义在成员位置：成员内部类、静态内部类

##### 局部内部类的使用

1. 内部类可以直接访问外部类的所有成员，包括私有的
2. 外部类访问局部内部类的成员需要创建对象才能访问，且必须在内部类的作用域内创建对象
3. 外部其他类不能访问局部内部类（局部变量）
4. 不能添加访问修饰符，因为它的地位就是一个局部变量，局部变量是不能使用访问修饰符的，但是可以用final修饰
5. 作用域：在定义它的代码块、方法体内
6. 外部类和内部类的成员重名时遵循就近原则，此时访问外部类的成员可以使用：外部类名.this.成员

##### 匿名内部类的使用

- 基于接口的匿名内部类

```java
class Outer{
    void method(){
        /*
        a的编译类型：Ia
        a的运行类型:Outer$1
        底层会分配类名Outer$1
        class Outer$1 implements Ia{
        	public void a(){}
        }
        底层创建匿名内部类Outer$1后立马就创建了Outer$1的实例对象，并把地址给了a
        匿名内部类只能使用一次
        */
        Ia a = new Ia(){//匿名内部类
            public void a(){
                
		   }
        };
    }
}
interface Ia{
    void a();
}
```

- 基于类的匿名内部类

```java
class Outer{
    void method(){
        /*
        a的编译类型A
        a的运行类型Outer$1
        底层会创建匿名内部类
        class Outer$1 extends A{
        	void aa(){}
        }
        */
    	A a = new A(){
            void test(){}
        };
    }
}
class A{
    
}
```

- 基于抽象类的匿名内部类

```java
class Outer{
    void method(){
        /*
        a的编译类型A
        a的运行类型Outer$1
        底层会创建匿名内部类
        class Outer$1 extends A{
        	void aa(){}
        }
        */
        A a = new A(){
            void aa(){
                
            }
        };
    }
}
abstract class A{
    abstract void a();
}
```

##### 成员内部类的使用

1. 可以直接访问外部类的所有成员，包括私有的
2. 外部类/外部其他类 访问成员内部类同样需要创建对象
3. 可以添加任意访问修饰符（它的地位就是一个成员）
4. 作用域同外部类的其他成员一样，为整个类体
5. 成员内部类中使用（外部类名.this.成员）区分重名成员

```java
package com.stu_cynbl;

/**
 * @author Roozen
 * @version 1.0
 */
public class Outer {
    //成员内部类
    class Inner{
        public void inner(){
            System.out.println("成员内部类");
        }
    }
    public void outer(){
        System.out.println("外部类");
    }

    public  Inner getInnerInstance(){
        //此处相当于是this.new Inner();
        //故此方法不能为static
        return new Inner();
    }
    public static Inner getInnerInstanceStatic(){
        //静态的必须先有外部类对象
        return new Outer().new Inner();
    }
}

class Other{
    public static void main(String[] args) {
        System.out.println("外部其他类");
        //外部其他类访问成员内部类的三种方式
        Outer.Inner a = new Outer().new Inner();
        a.inner();

        Outer b = new Outer();
        Outer.Inner b_inner = b.new Inner();
        b.outer();
        b_inner.inner();

        //在外部内中编写一个方法可以返回内部类对象
        Outer.Inner c = new Outer().getInnerInstance();
        c.inner();
        Outer.Inner d = Outer.getInnerInstanceStatic();
        d.inner();
    }
}
```

##### 静态内部类的使用

1. 静态内部类必须定义在外部类的成员位置，并且有static修饰
2. 可以直接访问外部类的所有静态成员，包含私有的，但不能直接访问非静态成员
3. 可以添加任意访问修饰符
4. 作用域为整个类体
5. 外部类/外部其他类访问静态内部类同样需要创建对象
6. 可以使用（外部类名.成员）来区分外部类与静态内部类的重名成员

```java
package com.stu_nbl.stu_jtnbl;

/**
 * @author Roozen
 * @version 1.0
 */
public class Outer {
    static class Inner{
        public void inner(){
            System.out.println("静态内部类");
        }
    }
    public void outer(){
        System.out.println("外部类");
    }

    public Inner getInnerInstance(){
        return new Outer.Inner();
    }
    public static Inner getInnerInstanceStatic(){
        return new Inner();
    }
}
class Other{
    public static void main(String[] args) {
        System.out.println("外部其他类");
        //外部其他类访问静态内部类

        //不需要创建外部类对象
        Outer.Inner a = new Outer.Inner();
        a.inner();
        //在外部内中编写一个方法可以返回内部类对象
        Outer.Inner b = Outer.getInnerInstanceStatic();
        Outer.Inner c = new Outer().getInnerInstance();

    }
}
```

## 6. 枚举（enumeration）

### 6.1 枚举的两种实现

1. 自定义

```java
/*
1.不需要提供set方法，枚举对象通常为只读
2.对枚举对象使用final static,实现底层优化
3.构造器私有化
4.对外暴露对象
*/

package com.hspedu.enum_;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class Enumeration02 {
    public static void main(String[] args) {
        System.out.println(Season.AUTUMN);
        System.out.println(Season.SPRING);
    }
}

//演示字定义枚举实现
class Season {//类
    private String name;
    private String desc;//描述

    //定义了四个对象, 固定.
    public static final Season SPRING = new Season("春天", "温暖");
    public static final Season WINTER = new Season("冬天", "寒冷");
    public static final Season AUTUMN = new Season("秋天", "凉爽");
    public static final Season SUMMER = new Season("夏天", "炎热");


    //1. 将构造器私有化,目的防止 直接 new
    //2. 去掉setXxx方法, 防止属性被修改
    //3. 在Season 内部，直接创建固定的对象
    //4. 优化，可以加入 final 修饰符
    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}

```



2. 使用enum

```java
/*
1.使用enum关键字定义枚举类时，默认会继承java.lang.Enum类,默认会被final修饰
2.传统的 public static final Season2 SPRING = new Season2("春天", "温暖"); 简化成 SPRING("春天", "温暖")，这里必须知道，它调用的是哪个构造器
3.如果使用无参构造器创建枚举对象，则实参列表和小括号都可以省略
4.当有多个枚举对象时，使用","间隔，最后有一个分号结尾
5.枚举对象必须放在枚举类的行首
*/

public class EnumExercise01 {
    public static void main(String[] args) {
        Gender2 boy = Gender2.BOY;//OK
        Gender2 boy2 = Gender2.BOY;//OK
        System.out.println(boy);//输出BOY 
        //本质就是调用 Gender2 的父类Enum的toString()
//        public String toString() {
//            return name;
//        }
        System.out.println(boy2 == boy);  //True
    }
}

enum Gender2{ //父类 Enum 的toString
    BOY , GIRL;
}
```

### 6.2 枚举类的常用方法

1. toString:Enum 类已经重写过了，返回的是当前对象名,子类可以重写该方法，用于返回对象的属性信息
2. name：返回当前对象名（常量名），子类中不能重写
3. ordinal：返回当前对象的位置号，默认从 0 开始
4. values：返回当前枚举类中所有的常量 
5. valueOf：将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报异常！
6. compareTo：比较两个枚举常量，比较的就是编号！

 ```java
 package com.hspedu.enum_;
 
 /**
  * @author 韩顺平
  * @version 1.0
  * 演示Enum类的各种方法的使用
  */
 public class EnumMethod {
     public static void main(String[] args) {
         //使用Season2 枚举类，来演示各种方法
         Season2 autumn = Season2.AUTUMN;
 
         //输出枚举对象的名字
         System.out.println(autumn.name());
         //ordinal() 输出的是该枚举对象的次序/编号，从0开始编号
         //AUTUMN 枚举对象是第三个，因此输出 2
         System.out.println(autumn.ordinal());
         //从反编译可以看出 values方法，返回 Season2[]
         //含有定义的所有枚举对象
         Season2[] values = Season2.values();
         System.out.println("===遍历取出枚举对象(增强for)====");
         for (Season2 season: values) {//增强for循环
             System.out.println(season);
         }
 
         //valueOf：将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报异常
         //执行流程
         //1. 根据你输入的 "AUTUMN" 到 Season2的枚举对象去查找
         //2. 如果找到了，就返回，如果没有找到，就报错
         Season2 autumn1 = Season2.valueOf("AUTUMN");
         System.out.println("autumn1=" + autumn1);
         System.out.println(autumn == autumn1);
 
         //compareTo：比较两个枚举常量，比较的就是编号
         //老韩解读
         //1. 就是把 Season2.AUTUMN 枚举对象的编号 和 Season2.SUMMER枚举对象的编号比较
         //2. 看看结果
         /*
         public final int compareTo(E o) {
 
             return self.ordinal - other.ordinal;
         }
         Season2.AUTUMN的编号[2] - Season2.SUMMER的编号[3]
          */
         System.out.println(Season2.AUTUMN.compareTo(Season2.SUMMER));
 
         //补充了一个增强for
 //        int[] nums = {1, 2, 9};
 //        //普通的for循环
 //        System.out.println("=====普通的for=====");
 //        for (int i = 0; i < nums.length; i++) {
 //            System.out.println(nums[i]);
 //        }
 //        System.out.println("=====增强的for=====");
 //        //执行流程是 依次从nums数组中取出数据，赋给i, 如果取出完毕，则退出for
 //        for(int i : nums) {
 //            System.out.println("i=" + i);
 //        }
     }
 }
 ```

### 6.3枚举类实现接口

>使用 enum 关键字后，就不能再继承其它类了，因为enum 会隐式继承 Enum，而 Java 是单继承机制。枚举类和普通类一样，可以实现接口，如下形式
>
>enum 类名 implements 接口 1，接口 2{} 

## 7. 注解（Annotation）

> 1. 注解(Annotation)也被称为元数据(Metadata)，用于修饰解释包、类、方法、属性、构造器、局部变量等数据息。
> 2. 和注释一样，注解不影响程序逻辑，但注解可以被编译或运行，相当于嵌入在代码中的补充信息。在 JavaSE 中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。
> 3. 在 JavaEE 中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替 java EE 旧版中所遗留的繁冗代码和 XML 配置等
> 4. 使用 Annotation 时要在其前面增加 @ 符号, 并把该 Annotation 当成一个修饰符使用。用于修饰它支持的程序元素

### 7.1 三个基本的注解

1. @Override: 限定某个方法，是重写父类方法, 该注解只能用于方法
2. @Deprecated: 用于表示某个程序元素(类, 方法等)已过时 
3. @SuppressWarnings: 抑制编译器警告 

```java
package com.hspedu.annotation_;

import java.util.ArrayList;
import java.util.List;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"rawtypes", "unchecked", "unused"})
public class SuppressWarnings_ {

    //老韩解读
    //1. 当我们不希望看到这些警告的时候，可以使用 SuppressWarnings注解来抑制警告信息
    //2. 在{""} 中，可以写入你希望抑制(不显示)警告信息
    //3. 可以指定的警告类型有
    //          all，抑制所有警告
    //          boxing，抑制与封装/拆装作业相关的警告
    //        //cast，抑制与强制转型作业相关的警告
    //        //dep-ann，抑制与淘汰注释相关的警告
    //        //deprecation，抑制与淘汰的相关警告
    //        //fallthrough，抑制与switch陈述式中遗漏break相关的警告
    //        //finally，抑制与未传回finally区块相关的警告
    //        //hiding，抑制与隐藏变数的区域变数相关的警告
    //        //incomplete-switch，抑制与switch陈述式(enum case)中遗漏项目相关的警告
    //        //javadoc，抑制与javadoc相关的警告
    //        //nls，抑制与非nls字串文字相关的警告
    //        //null，抑制与空值分析相关的警告
    //        //rawtypes，抑制与使用raw类型相关的警告
    //        //resource，抑制与使用Closeable类型的资源相关的警告
    //        //restriction，抑制与使用不建议或禁止参照相关的警告
    //        //serial，抑制与可序列化的类别遗漏serialVersionUID栏位相关的警告
    //        //static-access，抑制与静态存取不正确相关的警告
    //        //static-method，抑制与可能宣告为static的方法相关的警告
    //        //super，抑制与置换方法相关但不含super呼叫的警告
    //        //synthetic-access，抑制与内部类别的存取未最佳化相关的警告
    //        //sync-override，抑制因为置换同步方法而遗漏同步化的警告
    //        //unchecked，抑制与未检查的作业相关的警告
    //        //unqualified-field-access，抑制与栏位存取不合格相关的警告
    //        //unused，抑制与未用的程式码及停用的程式码相关的警告
    //4. 关于SuppressWarnings 作用范围是和你放置的位置相关
    //   比如 @SuppressWarnings放置在 main方法，那么抑制警告的范围就是 main
    //   通常我们可以放置具体的语句, 方法, 类.
    //5.  看看 @SuppressWarnings 源码
    //(1) 放置的位置就是 TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE
    //(2) 该注解类有数组 String[] values() 设置一个数组比如 {"rawtypes", "unchecked", "unused"}
    /*
        @Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
            @Retention(RetentionPolicy.SOURCE)
            public @interface SuppressWarnings {

                String[] value();
        }
     */
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("jack");
        list.add("tom");
        list.add("mary");
        int i;
        System.out.println(list.get(1));

    }

    public void f1() {
//        @SuppressWarnings({"rawtypes"})
        List list = new ArrayList();


        list.add("jack");
        list.add("tom");
        list.add("mary");
//        @SuppressWarnings({"unused"})
        int i;
        System.out.println(list.get(1));
    }
}

```

### 7.2 元注解(用于修饰其他注解)

1. Retention //指定注解的作用范围，三种SOURCE,CLASS,RUNTIME

> 只能用于修饰一个 Annotation 定义, 用于指定该 Annotation 可以保留多长时间, @Rentention 包含一个 RetentionPolicy 
>
> 类型的成员变量, 使用 @Rentention 时必须为该 value 成员变量指定值: 
>
> @Retention 的三种值 
>
> \1) RetentionPolicy.SOURCE: 编译器使用后，直接丢弃这种策略的注释 
>
> \2) RetentionPolicy.CLASS: 编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 不会保留注解。 这是默认值
>
> \3) RetentionPolicy.RUNTIME:编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 会保留注解. 程序可以通过反射获取该注解 

2. Target // 指定注解可以在哪些地方使用

3. Documented //指定该注解是否会在 javadoc 体现

4. Inherited //子类会继承父类注解

## 8. 异常

### 8.1 基本概念

![image-20220301195656619](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301195656619.png)

### 8.2异常体系图

![image-20220301195713611](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301195713611.png)

![image-20220301195739924](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301195739924.png)

### 8.3常见的运行时异常

1) NullPointerException 空指针异常
2) ArithmeticException 数学运算异常
3) ArrayIndexOutOfBoundsException 数组下标越界异常
4) ClassCastException 类型转换异常
5) NumberFormatException 数字格式不正确异常

### 8.4常见的编译时异常

![image-20220301200029017](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200029017.png)

![image-20220301200035236](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200035236.png)

### 8.5异常处理

![image-20220301200106395](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200106395.png)

![image-20220301200121736](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200121736.png)

![image-20220301200128427](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200128427.png)

![image-20220301200146065](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200146065.png)

![image-20220301200208804](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200208804.png)

![image-20220301200226082](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200226082.png)

![image-20220301200324161](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200324161.png)

![image-20220301200427719](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200427719.png)

![image-20220301200509861](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200509861.png)

![image-20220301200519156](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200519156.png)

![image-20220301200650655](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220301200650655.png)

### 8.6 自定义异常

![image-20220304105505223](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304105505223.png)

![image-20220304105539685](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304105539685.png)

```java
class AgeException extends RuntimeException {
	public AgeException(String message) {//构造器
		super(message);
	}
}
```

### 8.7 throw与throws

![image-20220304105700797](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304105700797.png)

## 9. 常用类

### 9.1 包装类Wrapper

#### 9.1.1 分类

![image-20220304205128082](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304205128082.png)

![image-20220304210351807](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304210351807.png)

![image-20220304210555733](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304210555733.png)

![image-20220304210625949](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304210625949.png)

#### 9.1.2 自动装箱/自动拆箱(jdk5及以后才有)

```java
package com.hspedu.wrapper;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class Integer01 {
    public static void main(String[] args) {
        //演示int <--> Integer 的装箱和拆箱
        //jdk5前是手动装箱和拆箱
        //手动装箱 int->Integer
        int n1 = 100;
        Integer integer = new Integer(n1);
        Integer integer1 = Integer.valueOf(n1);

        //手动拆箱
        //Integer -> int
        int i = integer.intValue();

        //jdk5后，就可以自动装箱和自动拆箱
        int n2 = 200;
        //自动装箱 int->Integer
        Integer integer2 = n2; //底层使用的是 Integer.valueOf(n2)
        //自动拆箱 Integer->int
        int n3 = integer2; //底层仍然使用的是 intValue()方法
    }
}
```

#### 9.1.3 包装类与String的转换

```java
package com.wrapper;

/**
 * @author Roozen
 * @version 1.0
 */
public class WrapperToString {
    public static void main(String[] args) {
        //包装类->String
        Integer i =100;

        //方式1
        String str1=i+"";
        //方式2
        String str2 = i.toString();
        //方式3(null安全的)
        //i为null时返回"null"
        String str3 = String.valueOf(i);

        //String -> 包装类
        String str4 = "12345";
        //方式1
        int i1=Integer.parseInt(str4);
        //方式2
        int i2 = new Integer(str4);
        //方式3
        int i3 = Integer.valueOf(str4);
    }
}
```

#### 9.1.4 Integer与Character的常用方法

```java
package com.hspedu.wrapper;

public class WrapperMethod {
    public static void main(String[] args) {
        System.out.println(Integer.MIN_VALUE); //返回最小值
        System.out.println(Integer.MAX_VALUE);//返回最大值

        System.out.println(Character.isDigit('a'));//判断是不是数字
        System.out.println(Character.isLetter('a'));//判断是不是字母
        System.out.println(Character.isUpperCase('a'));//判断是不是大写
        System.out.println(Character.isLowerCase('a'));//判断是不是小写

        System.out.println(Character.isWhitespace('a'));//判断是不是空格
        System.out.println(Character.toUpperCase('a'));//转成大写
        System.out.println(Character.toLowerCase('A'));//转成小写

    }
}
```

#### 9.1.5 面试题

```java
package com.wrapper;

public class test {
    public static void main(String[] args) {
        //三元运算符是一个整体，即Integer与Double做混合运算
        //Integer会先拆箱成int然后升级为double，最后装箱为Double
        Object obj = true ? new Integer(1):new Double(2.0);
        System.out.println(obj);//1.0

        //分支语句分别计算，不存在混合运算，也就不会转换
        Object obj2;
        if(true)
            obj2=new Integer(1);
        else {
            obj2 = new Double(2.0);
        }
        System.out.println(obj2);//1
    }
}
```

```java
package com.hspedu.wrapper;

public class WrapperExercise02 {
    public static void main(String[] args) {
        Integer i = new Integer(1);
        Integer j = new Integer(1);
        System.out.println(i == j);  //False
        //所以，这里主要是看范围 -128 ~ 127 就是直接返回
        /*
        老韩解读
        //1. 如果i 在 IntegerCache.low(-128)~IntegerCache.high(127),就直接从数组返回
        //2. 如果不在 -128~127,就直接 new Integer(i)
         public static Integer valueOf(int i) {
            if (i >= IntegerCache.low && i <= IntegerCache.high)
                return IntegerCache.cache[i + (-IntegerCache.low)];
            return new Integer(i);
        }
         */
        Integer m = 1; //底层 Integer.valueOf(1); -> 阅读源码
        Integer n = 1;//底层 Integer.valueOf(1);
        System.out.println(m == n); //T
        //所以，这里主要是看范围 -128 ~ 127 就是直接返回
        //，否则，就new Integer(xx);
        Integer x = 128;//底层Integer.valueOf(1);
        Integer y = 128;//底层Integer.valueOf(1);
        System.out.println(x == y);//False

    }
}
```

```java
package com.wrapper;

public class interview {
    public static void main(String[] args) {
        //示例一
        Integer i1 = new Integer(127);
        Integer i2 = new Integer(127);
        System.out.println(i1 == i2);//F
        //示例二
        Integer i3 = new Integer(128);
        Integer i4 = new Integer(128);
        System.out.println(i3 == i4);//F

        //示例三
        Integer i5 = 127;//底层Integer.valueOf(127)
        Integer i6 = 127;//-128~127
        System.out.println(i5 == i6); //T
        //示例四
        Integer i7 = 128;
        Integer i8 = 128;
        System.out.println(i7 == i8);//F
        //示例五
        Integer i9 = 127; //Integer.valueOf(127)
        Integer i10 = new Integer(127);
        System.out.println(i9 == i10);//F

        //示例六
        Integer i11 = 127;
        int i12 = 127;
        //只要有基本数据类型，判断的是值是否相同
        //这里的Integer自动拆箱成int了(i11 == i12.inValue())
        System.out.println(i11 == i12); //T
        //示例七
        Integer i13 = 128;
        int i14 = 128;
        System.out.println(i13 == i14);//T
    }
}
```

### 9.2 String类

#### 9.2.1 理解

```java
package com.hspedu.string_;

public class String01 {
    public static void main(String[] args) {
        //1.String 对象用于保存字符串，也就是一组字符序列
        //2. "jack" 字符串常量, 双引号括起的字符序列
        //3. 字符串的字符使用Unicode字符编码，一个字符(不区分字母还是汉字)占两个字节
        //4. String 类有很多构造器，构造器的重载
        //   常用的有 String  s1 = new String();
        //String  s2 = new String(String original);
        //String  s3 = new String(char[] a);
        //String  s4 =  new String(char[] a,int startIndex,int count)
        //String  s5 = new String(byte[] b)
        //5. String 类实现了接口 Serializable【String 可以串行化:可以在网络传输】
        //                 接口 Comparable [String 对象可以比较大小]
        //6. String 是final 类，不能被其他的类继承
        //7. String 有属性 private final char value[]; 用于存放字符串内容
        //8. 一定要注意：value 是一个final类型， 不可以修改(需要功力)：即value不能指向
        //   新的地址，但是单个字符内容是可以变化

        String name = "jack";
        name = "tom";
        final char[] value = {'a','b','c'};
        char[] v2 = {'t','o','m'};
        value[0] = 'H';
        //value = v2; 不可以修改 value地址

    }
}
```

#### 9.2.2 创建对象剖析

![image-20220304224656795](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304224656795.png)

![image-20220304224753121](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304224753121.png)

#### 9.2.3 字符串特性

![image-20220305083248290](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305083248290.png)

![image-20220305083405103](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305083405103.png)

![image-20220305083519521](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305083519521.png)

```java
package com.hspedu.string_;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class StringExercise08 {
    public static void main(String[] args) {
        String a = "hello"; //创建 a对象
        String b = "abc";//创建 b对象
        //老韩解读
        //1. 先 创建一个 StringBuilder sb = StringBuilder()
        //2. 执行  sb.append("hello");
        //3. sb.append("abc");
        //4. String c= sb.toString()
        //最后其实是 c 指向堆中的对象(String) value[] -> 池中 "helloabc"
        //字符串常量和变量相加也是如此
        String c = a + b;
        String d = "helloabc";
        System.out.println(c == d);//真还是假? 是false
        String e = "hello" + "abc";//直接看池， e指向常量池
        System.out.println(d == e);//真还是假? 是true
    }
}
```

![image-20220305084856786](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305084856786.png)

#### 9.2.4 常用方法

```java
package com.hspedu.string_;
/**
 * @author 韩顺平
 * @version 1.0
 */
public class StringMethod01 {
    public static void main(String[] args) {
        //1. equals 前面已经讲过了. 比较内容是否相同，区分大小写
        String str1 = "hello";
        String str2 = "Hello";
        System.out.println(str1.equals(str2));//

        // 2.equalsIgnoreCase 忽略大小写的判断内容是否相等
        String username = "johN";
        if ("john".equalsIgnoreCase(username)) {
            System.out.println("Success!");
        } else {
            System.out.println("Failure!");
        }
        
        // 3.length 获取字符的个数，字符串的长度
        System.out.println("韩顺平".length());
        // 4.indexOf 获取字符在字符串对象中第一次出现的索引，索引从0开始，如果找不到，返回-1
        String s1 = "wer@terwe@g";
        int index = s1.indexOf('@');
        System.out.println(index);// 3
        System.out.println("weIndex=" + s1.indexOf("we"));//0
        // 5.lastIndexOf 获取字符在字符串中最后一次出现的索引，索引从0开始，如果找不到，返回-1
        s1 = "wer@terwe@g@";
        index = s1.lastIndexOf('@');
        System.out.println(index);//11
        System.out.println("ter的位置=" + s1.lastIndexOf("ter"));//4
        // 6.substring 截取指定范围的子串
        String name = "hello,张三";
        //下面name.substring(6) 从索引6开始截取后面所有的内容
        System.out.println(name.substring(6));//截取后面的字符
        //name.substring(2,5)表示[2,5)
        System.out.println(name.substring(2,5));//llo
    }
}
```

```java
package com.hspedu.string_;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class StringMethod02 {
    public static void main(String[] args) {
        // 1.toUpperCase转换成大写
        String s = "heLLo";
        System.out.println(s.toUpperCase());//HELLO
        // 2.toLowerCase
        System.out.println(s.toLowerCase());//hello
        // 3.concat拼接字符串
        String s1 = "宝玉";
        s1 = s1.concat("林黛玉").concat("薛宝钗").concat("together");
        System.out.println(s1);//宝玉林黛玉薛宝钗together
        // 4.replace 替换字符串中的字符
        s1 = "宝玉 and 林黛玉 林黛玉 林黛玉";
        //在s1中，将 所有的 林黛玉 替换成薛宝钗
        // 老韩解读: s1.replace() 方法执行后，返回的结果才是替换过的.
        // 注意对 s1没有任何影响
        String s11 = s1.replace("宝玉", "jack");
        System.out.println(s1);//宝玉 and 林黛玉 林黛玉 林黛玉
        System.out.println(s11);//jack and 林黛玉 林黛玉 林黛玉
        // 5.split 分割字符串, 对于某些分割字符，我们需要 转义比如 | \\等
        String poem = "锄禾日当午,汗滴禾下土,谁知盘中餐,粒粒皆辛苦";
        //老韩解读：
        // 1. 以 , 为标准对 poem 进行分割 , 返回一个数组
        // 2. 在对字符串进行分割时，如果有特殊字符，需要加入 转义符 \
        String[] split = poem.split(",");
        poem = "E:\\aaa\\bbb";
        split = poem.split("\\\\");
        System.out.println("==分割后内容===");
        for (int i = 0; i < split.length; i++) {
            System.out.println(split[i]);
        }
        // 6.toCharArray 转换成字符数组
        s = "happy";
        char[] chs = s.toCharArray();
        for (int i = 0; i < chs.length; i++) {
            System.out.println(chs[i]);
        }
        // 7.compareTo 比较两个字符串的大小，如果前者大，
        // 则返回正数，后者大，则返回负数，如果相等，返回0
        // 老韩解读
        // (1) 如果长度相同，并且每个字符也相同，就返回 0
        // (2) 如果长度相同或者不相同，但是在进行比较时，可以区分大小
        //     就返回 if (c1 != c2) {
        //                return c1 - c2;
        //            }
        // (3) 如果前面的部分都相同，就返回 str1.len - str2.len
        String a = "jcck";// len = 3
        String b = "jack";// len = 4
        System.out.println(a.compareTo(b)); // 返回值是 'c' - 'a' = 2的值
// 8.format 格式字符串
        /* 占位符有:
         * %s 字符串 %c 字符 %d 整型 %.2f 浮点型
         *
         */
        String name = "john";
        int age = 10;
        double score = 56.857;
        char gender = '男';
        //将所有的信息都拼接在一个字符串.
        String info =
                "我的姓名是" + name + "年龄是" + age + ",成绩是" + score + "性别是" + gender + "。希望大家喜欢我！";

        System.out.println(info);

        //老韩解读
        //1. %s , %d , %.2f %c 称为占位符
        //2. 这些占位符由后面变量来替换
        //3. %s 表示后面由 字符串来替换
        //4. %d 是整数来替换
        //5. %.2f 表示使用小数来替换，替换后，只会保留小数点两位, 并且进行四舍五入的处理
        //6. %c 使用char 类型来替换
        String formatStr = "我的姓名是%s 年龄是%d，成绩是%.2f 性别是%c.希望大家喜欢我！";

        String info2 = String.format(formatStr, name, age, score, gender);

        System.out.println("info2=" + info2);
    }
}
```



#### 测试题

```java
package com.hspedu.string_;

public class StringExercise03 {
    public static void main(String[] args) {
        String a = "hsp"; //a 指向 常量池的 “hsp”
        String b =new String("hsp");//b 指向堆中对象
        System.out.println(a.equals(b)); //T
        System.out.println(a==b); //F
        //b.intern() 方法返回常量池地址
        System.out.println(a==b.intern()); //T
        System.out.println(b==b.intern()); //F
    }
}
```

![image-20220304232024054](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220304232024054.png)

### 9.3 StringBuffer类

#### 9.3.1 基本介绍

![image-20220305094203326](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305094203326.png)

![image-20220305100411964](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305100411964.png)

```java
public class StringBuffer01 {
    public static void main(String[] args) {
        //老韩解读
        //1. StringBuffer 的直接父类 是 AbstractStringBuilder
        //2. StringBuffer 实现了 Serializable, 即StringBuffer的对象可以串行化
        //3. 在父类中  AbstractStringBuilder 有属性 char[] value,不是final
        //   该 value 数组存放 字符串内容，引出存放在堆中的
        //4. StringBuffer 是一个 final类，不能被继承
        //5. 因为StringBuffer 字符内容是存在 char[] value, 所有在变化(增加/删除)
        //   不用每次都更换地址(即不是每次创建新对象)， 所以效率高于 String
        StringBuffer stringBuffer = new StringBuffer("hello");
    }
}
```

![image-20220305094329235](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305094329235.png)

#### 9.3.2 构造器

![image-20220305100328881](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305100328881.png)

```java
public class StringBuffer02 {
    public static void main(String[] args) {
        //构造器的使用
        //老韩解读
        //1. 创建一个 大小为 16的 char[] ,用于存放字符内容
        StringBuffer stringBuffer = new StringBuffer();
        //2 通过构造器指定 char[] 大小
        StringBuffer stringBuffer1 = new StringBuffer(100);
        //3. 通过 给一个String 创建 StringBuffer, char[] 大小就是 str.length() + 16
        StringBuffer hello = new StringBuffer("hello");
    }
}
```

#### 9.3.3 String和StrinBuffer转换

```java
public class StringAndStringBuffer {
    public static void main(String[] args) {
        //看 String——>StringBuffer
        String str = "hello tom";
        //方式1 使用构造器
        //注意： 返回的才是StringBuffer对象，对str 本身没有影响
        StringBuffer stringBuffer = new StringBuffer(str);
        //方式2 使用的是append方法
        StringBuffer stringBuffer1 = new StringBuffer();
        stringBuffer1 = stringBuffer1.append(str);
        //看看 StringBuffer ->String
        StringBuffer stringBuffer3 = new StringBuffer("韩顺平教育");
        //方式1 使用StringBuffer提供的 toString方法
        String s = stringBuffer3.toString();
        //方式2: 使用构造器来搞定
        String s1 = new String(stringBuffer3);
    }
}
```

#### 9.3.4 StringBuffer方法

```java
public class StringBufferMethod {
    public static void main(String[] args) {
        StringBuffer s = new StringBuffer("hello");
        //增
        s.append(',');// "hello,"
        s.append("张三丰");//"hello,张三丰"
        s.append("赵敏").append(100).append(true).append(10.5);//"hello,张三丰赵敏100true10.5"
        System.out.println(s);//"hello,张三丰赵敏100true10.5"

        //删
        /*
         * 删除索引为>=start && <end 处的字符
         * 解读: 删除 11~14的字符 [11, 14)
         */
        s.delete(11, 14);
        System.out.println(s);//"hello,张三丰赵敏true10.5"
        
        //改
        //老韩解读，使用 周芷若 替换 索引9-11的字符 [9,11)
        s.replace(9, 11, "周芷若");
        System.out.println(s);//"hello,张三丰周芷若true10.5"
        
        //查找指定的子串在字符串第一次出现的索引，如果找不到返回-1
        int indexOf = s.indexOf("张三丰");
        System.out.println(indexOf);//6
        
        //插
        //老韩解读，在索引为9的位置插入 "赵敏",原来索引为9的内容自动后移
        s.insert(9, "赵敏");
        System.out.println(s);//"hello,张三丰赵敏周芷若true10.5"
        //长度
        System.out.println(s.length());//22
        System.out.println(s);
    }
}
```

#### 测试题

```java
String str = null;
StringBuffer sb = new StringBuffer();
//底层调用的是AbstractStringBuilder的appendNull()方法，会把"null"追加进去
sb.append(str);
System.out.print(sb.length());//4
System.out.print(sb);//null
//抛出空指针异常,底层调用super(str.length()+16)
StringBuffer sb1 = new StringBuffer(str);
```

### 9.4 StringBuilder类

#### 9.4.1 基本介绍

![image-20220305111757248](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305111757248.png)

![image-20220305111817614](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305111817614.png)

![image-20220305113357248](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305113357248.png)

```java
package com.hspedu.stringbuilder_;

public class StringBuilder01 {
    public static void main(String[] args) {
        //老韩解读
        //1. StringBuilder 继承 AbstractStringBuilder 类
        //2. 实现了 Serializable ,说明StringBuilder对象是可以串行化(对象可以网络传输,可以保存到文件)
        //3. StringBuilder 是final类, 不能被继承
        //4. StringBuilder 对象字符序列仍然是存放在其父类 AbstractStringBuilder的 char[] value;
        //   因此，字符序列是堆中
        //5. StringBuilder 的方法，没有做互斥的处理,即没有synchronized 关键字,因此在单线程的情况下使用
        //   StringBuilder
        StringBuilder stringBuilder = new StringBuilder();
    }
}
```

![image-20220305113725466](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305113725466.png)

![image-20220305114127927](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305114127927.png)

### 9.5 Math类

```java
package com.hspedu.math_;

public class MathMethod {
    public static void main(String[] args) {
        //看看Math常用的方法(静态方法)
        //1.abs 绝对值
        int abs = Math.abs(-9);
        System.out.println(abs);//9
        //2.pow 求幂
        double pow = Math.pow(2, 4);//2的4次方
        System.out.println(pow);//16
        //3.ceil 向上取整,返回>=该参数的最小整数(转成double);
        double ceil = Math.ceil(3.9);
        System.out.println(ceil);//4.0
        //4.floor 向下取整，返回<=该参数的最大整数(转成double)
        double floor = Math.floor(4.001);
        System.out.println(floor);//4.0
        //5.round 四舍五入  Math.floor(该参数+0.5)
        long round = Math.round(5.51);
        System.out.println(round);//6
        //6.sqrt 求开方
        double sqrt = Math.sqrt(9.0);
        System.out.println(sqrt);//3.0

        //7.random 求随机数
        //  random 返回的是 0 <= x < 1 之间的一个随机小数
        // 思考：请写出获取 a-b之间的一个随机整数,a,b均为整数 ，比如 a = 2, b=7
        //  即返回一个数 x  2 <= x <= 7
        // 老韩解读 Math.random() * (b-a) 返回的就是 0  <= 数 <= b-a
        // (1) (int)(a) <= x <= (int)(a + Math.random() * (b-a +1) )
        // (2) 使用具体的数给小伙伴介绍 a = 2  b = 7
        //  (int)(a + Math.random() * (b-a +1) ) = (int)( 2 + Math.random()*6)
        //  Math.random()*6 返回的是 0 <= x < 6 小数
        //  2 + Math.random()*6 返回的就是 2<= x < 8 小数
        //  (int)(2 + Math.random()*6) = 2 <= x <= 7
        // (3) 公式就是  (int)(a + Math.random() * (b-a +1) )
        for(int i = 0; i < 100; i++) {
            System.out.println((int)(2 +  Math.random() * (7 - 2 + 1)));
        }

        //max , min 返回最大值和最小值
        int min = Math.min(1, 9);
        int max = Math.max(45, 90);
        System.out.println("min=" + min);
        System.out.println("max=" + max);
    }
}
```

### 9.6 Arrays类

```java
package com.hspedu.arrays_;

import java.util.Arrays;
import java.util.Comparator;

public class ArraysMethod01 {
    public static void main(String[] args) {

        Integer[] integers = {1, 20, 90};
        //遍历数组
//        for(int i = 0; i < integers.length; i++) {
//            System.out.println(integers[i]);
//        }
        //直接使用Arrays.toString方法，显示数组
//        System.out.println(Arrays.toString(integers));//

        //演示 sort方法的使用
        Integer arr[] = {1, -1, 7, 0, 89};
        //进行排序
        //老韩解读
        //1. 可以直接使用冒泡排序 , 也可以直接使用Arrays提供的sort方法排序
        //2. 因为数组是引用类型，所以通过sort排序后，会直接影响到 实参 arr
        //3. sort重载的，也可以通过传入一个接口 Comparator 实现定制排序
        //4. 调用 定制排序 时，传入两个参数 (1) 排序的数组 arr
        //   (2) 实现了Comparator接口的匿名内部类 , 要求实现  compare方法
        //5. 先演示效果，再解释
        //6. 这里体现了接口编程的方式 , 看看源码，就明白
        //   源码分析
        //(1) Arrays.sort(arr, new Comparator()
        //(2) 最终到 TimSort类的 private static <T> void binarySort(T[] a, int lo, int hi, int start,
        //                                       Comparator<? super T> c)()
        //(3) 执行到 binarySort方法的代码, 会根据动态绑定机制 c.compare()执行我们传入的
        //    匿名内部类的 compare ()
        //     while (left < right) {
        //                int mid = (left + right) >>> 1;
        //                if (c.compare(pivot, a[mid]) < 0)
        //                    right = mid;
        //                else
        //                    left = mid + 1;
        //            }
        //(4) new Comparator() {
        //            @Override
        //            public int compare(Object o1, Object o2) {
        //                Integer i1 = (Integer) o1;
        //                Integer i2 = (Integer) o2;
        //                return i2 - i1;
        //            }
        //        }
        //(5) public int compare(Object o1, Object o2) 返回的值>0 还是 <0
        //    会影响整个排序结果, 这就充分体现了 接口编程+动态绑定+匿名内部类的综合使用
        //    将来的底层框架和源码的使用方式，会非常常见
        //Arrays.sort(arr); // 默认排序方法
        //定制排序
        Arrays.sort(arr, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                Integer i1 = (Integer) o1;
                Integer i2 = (Integer) o2;
                return i2 - i1;
            }
        });
        System.out.println("===排序后===");
        System.out.println(Arrays.toString(arr));
    }
}
```

```java
package com.hspedu.arrays_;

import java.util.Arrays;
import java.util.List;

public class ArraysMethod02 {
    public static void main(String[] args) {
        Integer[] arr = {1, 2, 90, 123, 567};
        // binarySearch 通过二分搜索法进行查找，要求必须排好
        //1. 使用 binarySearch 二叉查找
        //2. 要求该数组是有序的. 如果该数组是无序的，不能使用binarySearch
        //3. 如果数组中不存在该元素，就返回 return -(low + 1);  // key not found.
        int index = Arrays.binarySearch(arr, 567);
        System.out.println("index=" + index);

        //copyOf 数组元素的复制
        // 老韩解读
        //1. 从 arr 数组中，拷贝 arr.length个元素到 newArr数组中
        //2. 如果拷贝的长度 > arr.length 就在新数组的后面 增加 null
        //3. 如果拷贝长度 < 0 就抛出异常NegativeArraySizeException
        //4. 该方法的底层使用的是 System.arraycopy()
        Integer[] newArr = Arrays.copyOf(arr, arr.length);
        System.out.println("==拷贝执行完毕后==");
        System.out.println(Arrays.toString(newArr));

        //fill 数组元素的填充
        Integer[] num = new Integer[]{9,3,2};
        //老韩解读
        //1. 使用 99 去填充 num数组，可以理解成是替换原理的元素
        Arrays.fill(num, 99);
        System.out.println("==num数组填充后==");
        System.out.println(Arrays.toString(num));

        //equals 比较两个数组元素内容是否完全一致
        Integer[] arr2 = {1, 2, 90, 123};
        //老韩解读
        //1. 如果arr 和 arr2 数组的元素一样，则方法true;
        //2. 如果不是完全一样，就返回 false
        boolean equals = Arrays.equals(arr, arr2);
        System.out.println("equals=" + equals);

        //asList 将一组值，转换成list
        //老韩解读
        //1. asList方法，会将 (2,3,4,5,6,1)数据转成一个List集合
        //2. 返回的 asList 编译类型 List(接口)
        //3. asList 运行类型 java.util.Arrays#ArrayList, 是Arrays类的
        //   静态内部类 private static class ArrayList<E> extends AbstractList<E>
        //              implements RandomAccess, java.io.Serializable
        List asList = Arrays.asList(2,3,4,5,6,1);
        System.out.println("asList=" + asList);
        System.out.println("asList的运行类型" + asList.getClass());
    }
}
```

### 9.7 System类

```java
package com.hspedu.system_;

import java.util.Arrays;

public class System_ {
    public static void main(String[] args) {
        //exit 退出当前程序
        System.out.println("ok1");
        //老韩解读
        //1. exit(0) 表示程序退出
        //2. 0 表示一个状态 , 正常的状态
        System.exit(0);//
        System.out.println("ok2");

        //arraycopy ：复制数组元素，比较适合底层调用，
        // 一般使用Arrays.copyOf完成复制数组

        int[] src={1,2,3};
        int[] dest = new int[3];// dest 当前是 {0,0,0}

        //老韩解读
        //1. 主要是搞清楚这五个参数的含义
        //2.
        //     源数组
        //     * @param      src      the source array.
        //     srcPos： 从源数组的哪个索引位置开始拷贝
        //     * @param      srcPos   starting position in the source array.
        //     dest : 目标数组，即把源数组的数据拷贝到哪个数组
        //     * @param      dest     the destination array.
        //     destPos: 把源数组的数据拷贝到 目标数组的哪个索引
        //     * @param      destPos  starting position in the destination data.
        //     length: 从源数组拷贝多少个数据到目标数组
        //     * @param      length   the number of array elements to be copied.
        System.arraycopy(src, 0, dest, 0, src.length);
        // int[] src={1,2,3};
        System.out.println("dest=" + Arrays.toString(dest));//[1, 2, 3]

        //currentTimeMillens:返回当前时间距离1970-1-1 的毫秒数
        // 老韩解读:
        System.out.println(System.currentTimeMillis());
    }
}
```

### 9.8 BidInteger类

```java
package com.hspedu.bignum;

import java.math.BigInteger;

public class BigInteger_ {
    public static void main(String[] args) {

        //当我们编程中，需要处理很大的整数，long 不够用
        //可以使用BigInteger的类来搞定
//        long l = 23788888899999999999999999999l;
//        System.out.println("l=" + l);

        //这里如果不用字符串依然会报错，因为字面量不支持这么大
        BigInteger bigInteger = new BigInteger("23788888899999999999999999999");
        BigInteger bigInteger2 = new BigInteger("10099999999999999999999999999999999999999999999999999999999999999999999999999999999");
        System.out.println(bigInteger);
        //老韩解读
        //1. 在对 BigInteger 进行加减乘除的时候，需要使用对应的方法，不能直接进行 + - * /
        //2. 可以创建一个 要操作的 BigInteger 然后进行相应操作
        BigInteger add = bigInteger.add(bigInteger2);
        System.out.println(add);//
        BigInteger subtract = bigInteger.subtract(bigInteger2);
        System.out.println(subtract);//减
        BigInteger multiply = bigInteger.multiply(bigInteger2);
        System.out.println(multiply);//乘
        BigInteger divide = bigInteger.divide(bigInteger2);
        System.out.println(divide);//除
    }
}
```

### 9.9 BigDecimal类

```java
package com.hspedu.bignum;

import java.math.BigDecimal;

public class BigDecimal_ {
    public static void main(String[] args) {
        //当我们需要保存一个精度很高的数时，double 不够用
        //可以是 BigDecimal
//        double d = 1999.11111111111999999999999977788d;
//        System.out.println(d);
        BigDecimal bigDecimal = new BigDecimal("1999.11");
        BigDecimal bigDecimal2 = new BigDecimal("3");
        System.out.println(bigDecimal);

        //老韩解读
        //1. 如果对 BigDecimal进行运算，比如加减乘除，需要使用对应的方法
        //2. 创建一个需要操作的 BigDecimal 然后调用相应的方法即可
        System.out.println(bigDecimal.add(bigDecimal2));
        System.out.println(bigDecimal.subtract(bigDecimal2));
        System.out.println(bigDecimal.multiply(bigDecimal2));
        //System.out.println(bigDecimal.divide(bigDecimal2));//可能抛出异常ArithmeticException
        //在调用divide 方法时，指定精度即可. BigDecimal.ROUND_CEILING
        //如果有无限循环小数，就会保留和分子一样的精度
        System.out.println(bigDecimal.divide(bigDecimal2, BigDecimal.ROUND_CEILING));
    }
}
```

### 9.10 日期类

#### 9.10.1 第一代日期类

![image-20220305170343654](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305170343654.png)

![image-20220305170622368](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305170622368.png)

```java
package com.hspedu.date_;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Date01 {
    public static void main(String[] args) throws ParseException {

        //老韩解读
        //1. 获取当前系统时间
        //2. 这里的Date 类是在java.util包
        //3. 默认输出的日期格式是国外的方式, 因此通常需要对格式进行转换
        Date d1 = new Date(); //获取当前系统时间
        System.out.println("当前日期=" + d1);
        Date d2 = new Date(9234567); //通过指定毫秒数得到时间
        System.out.println("d2=" + d2); //获取某个时间对应的毫秒数

        //老韩解读
        //1. 创建 SimpleDateFormat对象，可以指定相应的格式
        //2. 这里的格式使用的字母是规定好，不能乱写

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 hh:mm:ss E");
        String format = sdf.format(d1); // format:将日期转换成指定格式的字符串
        System.out.println("当前日期=" + format);

        //老韩解读
        //1. 可以把一个格式化的String 转成对应的 Date
        //2. 得到Date 仍然在输出时，还是按照国外的形式，如果希望指定格式输出，需要转换
        //3. 在把String -> Date ， 使用的 sdf 格式需要和你给的String的格式一样，否则会抛出转换异常
        String s = "1996年01月01日 10:20:30 星期一";
        Date parse = sdf.parse(s);
        System.out.println("parse=" + sdf.format(parse));
    }
}
```

#### 9.10.2 第二代日期类

![image-20220305171059880](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305171059880.png)

```java
package com.hspedu.date_;

import java.util.Calendar;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class Calendar_ {
    public static void main(String[] args) {
        //老韩解读
        //1. Calendar是一个抽象类， 并且构造器是private
        //2. 可以通过 getInstance() 来获取实例
        //3. 提供大量的方法和字段提供给程序员
        //4. Calendar没有提供对应的格式化的类，因此需要程序员自己组合来输出(灵活)
        //5. 如果我们需要按照 24小时进制来获取时间， Calendar.HOUR ==改成=> Calendar.HOUR_OF_DAY
        Calendar c = Calendar.getInstance(); //创建日历类对象//比较简单，自由
        System.out.println("c=" + c);
        //2.获取日历对象的某个日历字段
        System.out.println("年：" + c.get(Calendar.YEAR));
        // 这里为什么要 + 1, 因为Calendar 返回月时候，是按照 0 开始编号
        System.out.println("月：" + (c.get(Calendar.MONTH) + 1));
        System.out.println("日：" + c.get(Calendar.DAY_OF_MONTH));
        System.out.println("小时：" + c.get(Calendar.HOUR));
        System.out.println("分钟：" + c.get(Calendar.MINUTE));
        System.out.println("秒：" + c.get(Calendar.SECOND));
        //Calender 没有专门的格式化方法，所以需要程序员自己来组合显示
        System.out.println(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-" + c.get(Calendar.DAY_OF_MONTH) +
                " " + c.get(Calendar.HOUR_OF_DAY) + ":" + c.get(Calendar.MINUTE) + ":" + c.get(Calendar.SECOND) );

    }
}
```

#### 9.10.3 第三代日期类

![image-20220305173209304](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305173209304.png)

![image-20220305175727248](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305175727248.png)

![image-20220305180407903](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305180407903.png)

```java
package com.hspedu.date_;

import java.time.Instant;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.Collection;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class LocalDate_ {
    public static void main(String[] args) {
        //第三代日期
        //老韩解读
        //1. 使用now() 返回表示当前日期时间的 对象
        LocalDateTime ldt = LocalDateTime.now(); //LocalDate.now();//LocalTime.now()
        System.out.println(ldt);

        //2. 使用DateTimeFormatter 对象来进行格式化
        // 创建 DateTimeFormatter对象
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String format = dateTimeFormatter.format(ldt);
        System.out.println("格式化的日期=" + format);

        System.out.println("年=" + ldt.getYear());
        System.out.println("月=" + ldt.getMonth());//英文单词
        System.out.println("月=" + ldt.getMonthValue());//数字
        System.out.println("日=" + ldt.getDayOfMonth());
        System.out.println("时=" + ldt.getHour());
        System.out.println("分=" + ldt.getMinute());
        System.out.println("秒=" + ldt.getSecond());

        LocalDate now = LocalDate.now(); //可以获取年月日
        LocalTime now2 = LocalTime.now();//获取到时分秒


        //提供 plus 和 minus方法可以对当前时间进行加或者减
        //看看890天后，是什么时候 把 年月日-时分秒
        LocalDateTime localDateTime = ldt.plusDays(890);
        System.out.println("890天后=" + dateTimeFormatter.format(localDateTime));

        //看看在 3456分钟前是什么时候，把 年月日-时分秒输出
        LocalDateTime localDateTime2 = ldt.minusMinutes(3456);
        System.out.println("3456分钟前 日期=" + dateTimeFormatter.format(localDateTime2));
    }
}
```

![image-20220305183510911](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305183510911.png)

```java
package com.hspedu.date_;

import java.time.Instant;
import java.util.Date;

public class Instant_ {
    public static void main(String[] args) {

        //1.通过 静态方法 now() 获取表示当前时间戳的对象
        Instant now = Instant.now();
        System.out.println(now);
        //2. 通过 from 可以把 Instant转成 Date
        Date date = Date.from(now);
        //3. 通过 date的toInstant() 可以把 date 转成Instant对象
        Instant instant = date.toInstant();
    }
}
```

## 10. 集合

### 10.1 集合介绍

![image-20220305215105443](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305215105443.png)

![image-20220305220743731](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305220743731.png)

![image-20220305220837474](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305220837474.png)

```java
package com.hspedu.collection_;

import java.util.ArrayList;
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

public class Collection_ {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        //老韩解读
        //1. 集合主要是两组(单列集合 , 双列集合)
        //2. Collection 接口有两个重要的子接口 List Set , 他们的实现子类都是单列集合
        //3. Map 接口的实现子类 是双列集合，存放的 K-V
        //4. 把老师梳理的两张图记住
        //Collection
        //Map
        ArrayList arrayList = new ArrayList();
        arrayList.add("jack");
        arrayList.add("tom");

        HashMap hashMap = new HashMap();
        hashMap.put("NO1", "北京");
        hashMap.put("NO2", "上海");
    }
}
```

### 10.2 Collection接口实现类的特点

![image-20220305222210105](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305222210105.png)

![image-20220305222238897](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305222238897.png)

```java
package com.hspedu.collection_;

import java.util.ArrayList;
import java.util.List;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class CollectionMethod {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        List list = new ArrayList();
//        add:添加单个元素
        list.add("jack");
        list.add(10);//list.add(new Integer(10))
        list.add(true);
        System.out.println("list=" + list);
//        remove:删除指定元素
        //list.remove(0);//删除第一个元素
        list.remove(true);//指定删除某个元素
        System.out.println("list=" + list);
//        contains:查找元素是否存在
        System.out.println(list.contains("jack"));//T
//        size:获取元素个数
        System.out.println(list.size());//2
//        isEmpty:判断是否为空
        System.out.println(list.isEmpty());//F
//        clear:清空
        list.clear();
        System.out.println("list=" + list);
//        addAll:添加多个元素
        ArrayList list2 = new ArrayList();
        list2.add("红楼梦");
        list2.add("三国演义");
        list.addAll(list2);
        System.out.println("list=" + list);
//        containsAll:查找多个元素是否都存在
        System.out.println(list.containsAll(list2));//T
//        removeAll：删除多个元素
        list.add("聊斋");
        list.removeAll(list2);
        System.out.println("list=" + list);//[聊斋]
//        说明：以ArrayList实现类来演示.
    }
}
```

![image-20220305223239120](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305223239120.png)

![image-20220305223327209](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305223327209.png)

![image-20220305223348850](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305223348850.png)

```java
package com.hspedu.collection_;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class CollectionIterator {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {

        Collection col = new ArrayList();
        col.add(new Book("三国演义", "罗贯中", 10.1));
        col.add(new Book("小李飞刀", "古龙", 5.1));
        col.add(new Book("红楼梦", "曹雪芹", 34.6));

        //System.out.println("col=" + col);
        //现在老师希望能够遍历 col集合
        //1. 先得到 col 对应的 迭代器
        Iterator iterator = col.iterator();
        //2. 使用while循环遍历
//        while (iterator.hasNext()) {//判断是否还有数据
//            //返回下一个元素，类型是Object
//            Object obj = iterator.next();
//            System.out.println("obj=" + obj);
//        }
        //老师教大家一个快捷键，快速生成 while => itit
        //显示所有的快捷键的的快捷键 ctrl + j
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println("obj=" + obj);
        }
        //3. 当退出while循环后 , 这时iterator迭代器，指向最后的元素
        //   iterator.next();//NoSuchElementException
        //4. 如果希望再次遍历，需要重置我们的迭代器
        iterator = col.iterator();
        System.out.println("===第二次遍历===");
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println("obj=" + obj);
        }

    }
}

class Book {
    private String name;
    private String author;
    private double price;

    public Book(String name, String author, double price) {
        this.name = name;
        this.author = author;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", author='" + author + '\'' +
                ", price=" + price +
                '}';
    }
}
```

![image-20220305224258529](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305224258529.png)

```java
package com.hspedu.collection_;

import java.util.ArrayList;
import java.util.Collection;

public class CollectionFor {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        Collection col = new ArrayList();

        col.add(new Book("三国演义", "罗贯中", 10.1));
        col.add(new Book("小李飞刀", "古龙", 5.1));
        col.add(new Book("红楼梦", "曹雪芹", 34.6));
        //老韩解读
        //1. 使用增强for, 在Collection集合
        //2. 增强for， 底层仍然是迭代器
        //3. 增强for可以理解成就是简化版本的 迭代器遍历
        //4. 快捷键方式 I
//        for (Object book : col) {
//            System.out.println("book=" + book);
//        }
        for (Object o : col) {
            System.out.println("book=" + o);
        }
        //增强for，也可以直接在数组使用
//        int[] nums = {1, 8, 10, 90};
//        for (int i : nums) {
//            System.out.println("i=" + i);
//        }
        
        //普通for也可
        for(int i = 0; i<col.size(); i++){
            System.out.println(col.get(i));
        }
    }
}
```

#### 10.2.1 List接口实现类的特点

![image-20220305225313580](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305225313580.png)

![image-20220305231033340](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305231033340.png)

```java
package com.hspedu.list_;

import java.util.ArrayList;
import java.util.List;

public class ListMethod {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        List list = new ArrayList();
        list.add("张三丰");
        list.add("贾宝玉");
//        void add(int index, Object ele):在index位置插入ele元素
        //在index = 1的位置插入一个对象
        list.add(1, "韩顺平");
        System.out.println("list=" + list);
//        boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
        List list2 = new ArrayList();
        list2.add("jack");
        list2.add("tom");
        list.addAll(1, list2);
        System.out.println("list=" + list);
//        Object get(int index):获取指定index位置的元素
        //说过
//        int indexOf(Object obj):返回obj在集合中首次出现的位置
        System.out.println(list.indexOf("tom"));//2
//        int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
        list.add("韩顺平");
        System.out.println("list=" + list);
        System.out.println(list.lastIndexOf("韩顺平"));
//        Object remove(int index):移除指定index位置的元素，并返回此元素
        list.remove(0);
        System.out.println("list=" + list);
//        Object set(int index, Object ele):设置指定index位置的元素为ele , 相当于是替换.
        list.set(1, "玛丽");
        System.out.println("list=" + list);
//        List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合
        // 注意返回的子集合 fromIndex <= subList < toIndex
        List returnlist = list.subList(0, 2);
        System.out.println("returnlist=" + returnlist);

    }
}
```

##### ArrayList

![image-20220305235248962](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220305235248962.png)

![image-20220306101302465](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306101302465.png)

##### Vector

![image-20220306113155953](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306113155953.png)

![image-20220306113443376](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306113443376.png)

```java
package com.hspedu.list_;

import java.util.Vector;

@SuppressWarnings({"all"})
public class Vector_ {
    public static void main(String[] args) {
        //无参构造器
        //有参数的构造
        Vector vector = new Vector(8);
        for (int i = 0; i < 10; i++) {
            vector.add(i);
        }
        vector.add(100);
        System.out.println("vector=" + vector);
        //老韩解读源码
        //1. new Vector() 底层
        /*
            public Vector() {
                this(10);
            }
         补充：如果是  Vector vector = new Vector(8);
            走的方法:
            public Vector(int initialCapacity) {
                this(initialCapacity, 0);
            }
         2. vector.add(i)
         2.1  //下面这个方法就添加数据到vector集合
            public synchronized boolean add(E e) {
                modCount++;
                ensureCapacityHelper(elementCount + 1);
                elementData[elementCount++] = e;
                return true;
            }
          2.2  //确定是否需要扩容 条件 ： minCapacity - elementData.length>0
            private void ensureCapacityHelper(int minCapacity) {
                // overflow-conscious code
                if (minCapacity - elementData.length > 0)
                    grow(minCapacity);
            }
          2.3 //如果 需要的数组大小 不够用，就扩容 , 扩容的算法
              //newCapacity = oldCapacity + ((capacityIncrement > 0) ?
              //                             capacityIncrement : oldCapacity);
              //就是扩容两倍.
            private void grow(int minCapacity) {
                // overflow-conscious code
                int oldCapacity = elementData.length;
                int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                                 capacityIncrement : oldCapacity);
                if (newCapacity - minCapacity < 0)
                    newCapacity = minCapacity;
                if (newCapacity - MAX_ARRAY_SIZE > 0)
                    newCapacity = hugeCapacity(minCapacity);
                elementData = Arrays.copyOf(elementData, newCapacity);
            }
         */
    }
}
```

##### LinkedList

![image-20220306114852039](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306114852039.png)

![image-20220306115204089](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306115204089.png)

```java
package com.hspedu.list_;

import java.util.Iterator;
import java.util.LinkedList;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class LinkedListCRUD {
    public static void main(String[] args) {

        LinkedList linkedList = new LinkedList();
        linkedList.add(1);
        linkedList.add(2);
        linkedList.add(3);
        System.out.println("linkedList=" + linkedList);

        //演示一个删除结点的
        linkedList.remove(); // 这里默认删除的是第一个结点
        //linkedList.remove(2);

        System.out.println("linkedList=" + linkedList);

        //修改某个结点对象
        linkedList.set(1, 999);
        System.out.println("linkedList=" + linkedList);

        //得到某个结点对象
        //get(1) 是得到双向链表的第二个对象
        Object o = linkedList.get(1);
        System.out.println(o);//999

        //因为LinkedList 实现了List接口, 遍历方式
        System.out.println("===LinkedList遍历迭代器====");
        Iterator iterator = linkedList.iterator();
        while (iterator.hasNext()) {
            Object next =  iterator.next();
            System.out.println("next=" + next);

        }

        System.out.println("===LinkeList遍历增强for====");
        for (Object o1 : linkedList) {
            System.out.println("o1=" + o1);
        }
        System.out.println("===LinkeList遍历普通for====");
        for (int i = 0; i < linkedList.size(); i++) {
            System.out.println(linkedList.get(i));
        }


        //老韩源码阅读.
        /* 1. LinkedList linkedList = new LinkedList();
              public LinkedList() {}
           2. 这时 linkeList 的属性 first = null  last = null
           3. 执行 添加
               public boolean add(E e) {
                    linkLast(e);
                    return true;
                }
            4.将新的结点，加入到双向链表的最后
             void linkLast(E e) {
                final Node<E> l = last;
                final Node<E> newNode = new Node<>(l, e, null);
                last = newNode;
                if (l == null)
                    first = newNode;
                else
                    l.next = newNode;
                size++;
                modCount++;
            }

         */

        /*
          老韩读源码 linkedList.remove(); // 这里默认删除的是第一个结点
          1. 执行 removeFirst
            public E remove() {
                return removeFirst();
            }
         2. 执行
            public E removeFirst() {
                final Node<E> f = first;
                if (f == null)
                    throw new NoSuchElementException();
                return unlinkFirst(f);
            }
          3. 执行 unlinkFirst, 将 f 指向的双向链表的第一个结点拿掉
            private E unlinkFirst(Node<E> f) {
                // assert f == first && f != null;
                final E element = f.item;
                final Node<E> next = f.next;
                f.item = null;
                f.next = null; // help GC
                first = next;
                if (next == null)
                    last = null;
                else
                    next.prev = null;
                size--;
                modCount++;
                return element;
            }
         */
    }
}
```

![image-20220306133151548](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306133151548.png)

#### 10.2.2 Set接口实现类的特点



![image-20220306133421854](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306133421854.png)

![image-20220306133445368](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306133445368.png)

```java
package com.hspedu.set_;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

@SuppressWarnings({"all"})
public class SetMethod {
    public static void main(String[] args) {
        //老韩解读
        //1. 以Set 接口的实现类 HashSet 来讲解Set 接口的方法
        //2. set 接口的实现类的对象(Set接口对象), 不能存放重复的元素, 可以添加一个null
        //3. set 接口对象存放数据是无序(即添加的顺序和取出的顺序不一致)
        //4. 注意：取出的顺序的顺序虽然不是添加的顺序，但是顺序是固定的.
        Set set = new HashSet();
        set.add("john");
        set.add("lucy");
        set.add("john");//重复
        set.add("jack");
        set.add("hsp");
        set.add("mary");
        set.add(null);//
        set.add(null);//再次添加null
        for(int i = 0; i <10;i ++) {
            System.out.println("set=" + set);
        }

        //遍历
        //方式1： 使用迭代器
        System.out.println("=====使用迭代器====");
        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            Object obj =  iterator.next();
            System.out.println("obj=" + obj);

        }

        set.remove(null);

        //方式2: 增强for
        System.out.println("=====增强for====");

        for (Object o : set) {
            System.out.println("o=" + o);
        }
        //set 接口对象，不能通过索引来获取
    }
}
```

##### HashSet

![image-20220306224142163](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220306224142163.png)

![image-20220307001719123](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220307001719123.png)

![image-20220307002010352](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220307002010352.png)

```java
package com.hspedu.set_;

import java.util.HashSet;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class HashSetSource {
    public static void main(String[] args) {

        HashSet hashSet = new HashSet();
        hashSet.add("java");//到此位置，第1次add分析完毕.
        hashSet.add("php");//到此位置，第2次add分析完毕
        hashSet.add("java");
        System.out.println("set=" + hashSet);

        /*
        老韩对HashSet 的源码解读
        1. 执行 HashSet()
            public HashSet() {
                map = new HashMap<>();
            }
        2. 执行 add()
           public boolean add(E e) {//e = "java"
                return map.put(e, PRESENT)==null;//(static) PRESENT = new Object();
           }
         3.执行 put() , 该方法会执行 hash(key) 得到key对应的hash值 算法h = key.hashCode()) ^ (h >>> 16)
             public V put(K key, V value) {//key = "java" value = PRESENT 共享
                return putVal(hash(key), key, value, false, true);
            }
         4.执行 putVal
         final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
                Node<K,V>[] tab; Node<K,V> p; int n, i; //定义了辅助变量
                //table 就是 HashMap 的一个数组，类型是 Node[]
                //if 语句表示如果当前table 是null, 或者 大小=0
                //就是第一次扩容，到16个空间.
                if ((tab = table) == null || (n = tab.length) == 0)
                    n = (tab = resize()).length;

                //(1)根据key，得到hash 去计算该key应该存放到table表的哪个索引位置
                //并把这个位置的对象，赋给 p
                //(2)判断p 是否为null
                //(2.1) 如果p 为null, 表示还没有存放元素, 就创建一个Node (key="java",value=PRESENT)
                //(2.2) 就放在该位置 tab[i] = newNode(hash, key, value, null)

                if ((p = tab[i = (n - 1) & hash]) == null)
                    tab[i] = newNode(hash, key, value, null);
                else {
                    //一个开发技巧提示： 在需要局部变量(辅助变量)时候，再创建
                    Node<K,V> e; K k; 
                    //如果当前索引位置对应的链表的第一个元素和准备添加的key的hash值一样
                    //并且满足 下面两个条件之一:
                    //(1) 准备加入的key 和 p 指向的Node 结点的 key 是同一个对象
                    //(2)  p 指向的Node 结点的 key 的equals() 和准备加入的key比较后相同
                    //就不能加入
                    if (p.hash == hash &&
                        ((k = p.key) == key || (key != null && key.equals(k))))
                        e = p;
                    //再判断 p 是不是一颗红黑树,
                    //如果是一颗红黑树，就调用 putTreeVal , 来进行添加
                    else if (p instanceof TreeNode)
                        e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
                    else {//如果table对应索引位置，已经是一个链表, 就使用for循环比较
                          //(1) 依次和该链表的每一个元素比较后，都不相同, 则加入到该链表的最后
                          //    注意在把元素添加到链表后，立即判断 该链表是否已经达到8个结点
                          //    , 就调用 treeifyBin() 对当前这个链表进行树化(转成红黑树)
                          //    注意，在转成红黑树时，要进行判断, 判断条件
                          //    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY(64))
                          //            resize();
                          //    如果上面条件成立，先table扩容.
                          //    只有上面条件不成立时，才进行转成红黑树
                          //(2) 依次和该链表的每一个元素比较过程中，如果有相同情况,就直接break

                            for (int binCount = 0; ; ++binCount) {
                                if ((e = p.next) == null) {
                                    p.next = newNode(hash, key, value, null);
                                    if (binCount >= TREEIFY_THRESHOLD(8) - 1) // -1 for 1st
                                        treeifyBin(tab, hash);
                                    break;
                                }
                                if (e.hash == hash &&
                                    ((k = e.key) == key || (key != null && key.equals(k))))
                                    break;
                                p = e;
                            }
                    	}
                    if (e != null) { // existing mapping for key
                        V oldValue = e.value;
                        if (!onlyIfAbsent || oldValue == null)
                            e.value = value;
                        afterNodeAccess(e);
                        return oldValue;
                    }
                }
                ++modCount;
                //size 就是我们每加入一个结点Node(k,v,h,next), size++
                if (++size > threshold)
                    resize();//扩容
                afterNodeInsertion(evict);
                return null;
            }
         */

    }
}
```

```java
package com.hspedu.set_;

import java.util.HashSet;
import java.util.Objects;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class HashSetIncrement {
    public static void main(String[] args) {
        /*
        HashSet底层是HashMap, 第一次添加时，table 数组扩容到 16，
        临界值(threshold)是 16*加载因子(loadFactor)是0.75 = 12
        如果table 数组使用到了临界值 12,就会扩容到 16 * 2 = 32,
        新的临界值就是 32*0.75 = 24, 依次类推

         */
        HashSet hashSet = new HashSet();
//        for(int i = 1; i <= 100; i++) {
//            hashSet.add(i);//1,2,3,4,5...100
//        }
        /*
        在Java8中, 如果一条链表的元素个数到达 TREEIFY_THRESHOLD(默认是 8 )，
        并且table的大小 >= MIN_TREEIFY_CAPACITY(默认64),就会进行树化(红黑树),
        否则仍然采用数组扩容机制

         */

//        for(int i = 1; i <= 12; i++) {
//            hashSet.add(new A(i));//
//        }


        /*
            当我们向hashset增加一个元素，-> Node -> 加入table , 就算是增加了一个size++

         */

        for(int i = 1; i <= 7; i++) {//在table的某一条链表上添加了 7个A对象
            hashSet.add(new A(i));//
        }

        for(int i = 1; i <= 7; i++) {//在table的另外一条链表上添加了 7个B对象
            hashSet.add(new B(i));//
        }



    }
}

class B {
    private int n;

    public B(int n) {
        this.n = n;
    }
    @Override
    public int hashCode() {
        return 200;
    }
}

class A {
    private int n;

    public A(int n) {
        this.n = n;
    }
    @Override
    public int hashCode() {
        return 100;
    }
}
```

##### LinkedHashSet

![image-20220307005113466](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220307005113466.png)

![image-20220307122848578](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220307122848578.png)

```java
package com.hspedu.set_;

import java.util.LinkedHashSet;
import java.util.Set;

@SuppressWarnings({"all"})
public class LinkedHashSetSource {
    public static void main(String[] args) {
        //分析一下LinkedHashSet的底层机制
        Set set = new LinkedHashSet();
        set.add(new String("AA"));
        set.add(456);
        set.add(456);
        set.add(new Customer("刘", 1001));
        set.add(123);
        set.add("HSP");

        System.out.println("set=" + set);
        //老韩解读
        //1. LinkedHashSet 加入顺序和取出元素/数据的顺序一致
        //2. LinkedHashSet 底层维护的是一个LinkedHashMap(是HashMap的子类)
        //3. LinkedHashSet 底层结构 (数组table+双向链表)
        //4. 添加第一次时，直接将 数组table 扩容到 16 ,存放的结点类型是 LinkedHashMap$Entry
        //5. 数组是 HashMap$Node[] 存放的元素/数据是 LinkedHashMap$Entry类型
        /*
                //继承关系是在内部类完成.
                static class Entry<K,V> extends HashMap.Node<K,V> {
                    Entry<K,V> before, after;
                    Entry(int hash, K key, V value, Node<K,V> next) {
                        super(hash, key, value, next);
                    }
                }

         */
    }
}
class Customer {
    private String name;
    private int no;

    public Customer(String name, int no) {
        this.name = name;
        this.no = no;
    }
}
```

##### TreeSet

```java
package com.hspedu.set_;

import java.util.Comparator;
import java.util.TreeSet;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class TreeSet_ {
    public static void main(String[] args) {

        //老韩解读
        //1. 当我们使用无参构造器，创建TreeSet时，仍然是无序的
        //2. 老师希望添加的元素，按照字符串大小来排序
        //3. 使用TreeSet 提供的一个构造器，可以传入一个比较器(匿名内部类)
        //   并指定排序规则
        //4. 简单看看源码
        //老韩解读
        /*
        1. 构造器把传入的比较器对象，赋给了 TreeSet的底层的 TreeMap的属性this.comparator

         public TreeMap(Comparator<? super K> comparator) {
                this.comparator = comparator;
            }
         2. 在 调用 treeSet.add("tom"), 在底层会执行到

             if (cpr != null) {//cpr 就是我们的匿名内部类(对象)
                do {
                    parent = t;
                    //动态绑定到我们的匿名内部类(对象)compare
                    cmp = cpr.compare(key, t.key);
                    if (cmp < 0)
                        t = t.left;
                    else if (cmp > 0)
                        t = t.right;
                    else //如果相等，即返回0,这个Key就没有加入
                        return t.setValue(value);
                } while (t != null);
            }
         */

//        TreeSet treeSet = new TreeSet();
        TreeSet treeSet = new TreeSet(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                //下面 调用String的 compareTo方法进行字符串大小比较
                //如果老韩要求加入的元素，按照长度大小排序
                //return ((String) o2).compareTo((String) o1);
                return ((String) o1).length() - ((String) o2).length();
            }
        });
        //添加数据.
        treeSet.add("jack");
        treeSet.add("tom");//3
        treeSet.add("sp");
        treeSet.add("a");
        treeSet.add("abc");//3

        System.out.println("treeSet=" + treeSet);
    }
}
```



### 10.3 Map

![image-20220307125545459](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220307125545459.png)

```java
package com.hspedu.map_;

import java.util.HashMap;
import java.util.Map;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class Map_ {
    public static void main(String[] args) {
        //老韩解读Map 接口实现类的特点, 使用实现类HashMap
        //1. Map与Collection并列存在。用于保存具有映射关系的数据:Key-Value(双列元素)
        //2. Map 中的 key 和  value 可以是任何引用类型的数据，会封装到HashMap$Node 对象中
        //3. Map 中的 key 不允许重复，原因和HashSet 一样，前面分析过源码.
        //4. Map 中的 value 可以重复
        //5. Map 的key 可以为 null, value 也可以为null ，注意 key 为null,
        //   只能有一个，value 为null ,可以多个
        //6. 常用String类作为Map的 key
        //7. key 和 value 之间存在单向一对一关系，即通过指定的 key 总能找到对应的 value
        Map map = new HashMap();
        map.put("no1", "韩顺平");//k-v
        map.put("no2", "张无忌");//k-v
        map.put("no1", "张三丰");//当有相同的k , 就等价于替换.
        map.put("no3", "张三丰");//k-v
        map.put(null, null); //k-v
        map.put(null, "abc"); //等价替换
        map.put("no4", null); //k-v
        map.put("no5", null); //k-v
        map.put(1, "赵敏");//k-v
        map.put(new Object(), "金毛狮王");//k-v
        // 通过get 方法，传入 key ,会返回对应的value
        System.out.println(map.get("no2"));//张无忌

        System.out.println("map=" + map);



    }
}

```



![image-20220307130130538](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220307130130538.png)

```java
package com.hspedu.map_;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class MapSource_ {
    public static void main(String[] args) {
        Map map = new HashMap();
        map.put("no1", "韩顺平");//k-v
        map.put("no2", "张无忌");//k-v
        map.put(new Car(), new Person());//k-v

        //老韩解读
        //1. k-v 最后是 HashMap$Node node = newNode(hash, key, value, null)
        //2. k-v 为了方便程序员的遍历，还会 创建 EntrySet 集合 ，该集合存放的元素的类型 Entry, 而一个Entry
        //   对象就有k,v EntrySet<Entry<K,V>> 即： transient Set<Map.Entry<K,V>> entrySet;
        //3. entrySet 中， 定义的类型是 Map.Entry ，但是实际上存放的还是 HashMap$Node
        //   这时因为 static class Node<K,V> implements Map.Entry<K,V>
        //4. 当把 HashMap$Node 对象 存放到 entrySet 就方便我们的遍历, 因为 Map.Entry 提供了重要方法
        //   K getKey(); V getValue();

        Set set = map.entrySet();
        System.out.println(set.getClass());// HashMap$EntrySet
        for (Object obj : set) {

            //System.out.println(obj.getClass()); //HashMap$Node
            //为了从 HashMap$Node 取出k-v
            //1. 先做一个向下转型
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "-" + entry.getValue() );
        }

        Set set1 = map.keySet();
        System.out.println(set1.getClass());
        Collection values = map.values();
        System.out.println(values.getClass());


    }
}

class Car {

}

class Person{

}

```

![image-20220307132006915](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220307132006915.png)

```java
package com.hspedu.map_;

import java.util.HashMap;
import java.util.Map;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class MapMethod {
    public static void main(String[] args) {
        //演示map接口常用方法

        Map map = new HashMap();
        map.put("邓超", new Book("", 100));//OK
        map.put("邓超", "孙俪");//替换-> 一会分析源码
        map.put("王宝强", "马蓉");//OK
        map.put("宋喆", "马蓉");//OK
        map.put("刘令博", null);//OK
        map.put(null, "刘亦菲");//OK
        map.put("鹿晗", "关晓彤");//OK
        map.put("hsp", "hsp的老婆");

        System.out.println("map=" + map);

//        remove:根据键删除映射关系
        map.remove(null);
        System.out.println("map=" + map);
//        get：根据键获取值
        Object val = map.get("鹿晗");
        System.out.println("val=" + val);
//        size:获取元素个数
        System.out.println("k-v=" + map.size());
//        isEmpty:判断个数是否为0
        System.out.println(map.isEmpty());//F
//        clear:清除k-v
        //map.clear();
        System.out.println("map=" + map);
//        containsKey:查找键是否存在
        System.out.println("结果=" + map.containsKey("hsp"));//T


    }
}

class Book {
    private String name;
    private int num;

    public Book(String name, int num) {
        this.name = name;
        this.num = num;
    }
}

```



![image-20220308001423839](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308001423839.png)

```java
package com.hspedu.map_;

import java.util.*;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class MapFor {
    public static void main(String[] args) {

        Map map = new HashMap();
        map.put("邓超", "孙俪");
        map.put("王宝强", "马蓉");
        map.put("宋喆", "马蓉");
        map.put("刘令博", null);
        map.put(null, "刘亦菲");
        map.put("鹿晗", "关晓彤");

        //第一组: 先取出 所有的Key , 通过Key 取出对应的Value
        Set keyset = map.keySet();
        //(1) 增强for
        System.out.println("-----第一种方式-------");
        for (Object key : keyset) {
            System.out.println(key + "-" + map.get(key));
        }
        //(2) 迭代器
        System.out.println("----第二种方式--------");
        Iterator iterator = keyset.iterator();
        while (iterator.hasNext()) {
            Object key =  iterator.next();
            System.out.println(key + "-" + map.get(key));
        }

        //第二组: 把所有的values取出
        Collection values = map.values();
        //这里可以使用所有的Collections使用的遍历方法
        //(1) 增强for
        System.out.println("---取出所有的value 增强for----");
        for (Object value : values) {
            System.out.println(value);
        }
        //(2) 迭代器
        System.out.println("---取出所有的value 迭代器----");
        Iterator iterator2 = values.iterator();
        while (iterator2.hasNext()) {
            Object value =  iterator2.next();
            System.out.println(value);

        }

        //第三组: 通过EntrySet 来获取 k-v
        Set entrySet = map.entrySet();// EntrySet<Map.Entry<K,V>>
        //(1) 增强for
        System.out.println("----使用EntrySet 的 for增强(第3种)----");
        for (Object entry : entrySet) {
            //将entry 转成 Map.Entry
            Map.Entry m = (Map.Entry) entry;
            System.out.println(m.getKey() + "-" + m.getValue());
        }
        //(2) 迭代器
        System.out.println("----使用EntrySet 的 迭代器(第4种)----");
        Iterator iterator3 = entrySet.iterator();
        while (iterator3.hasNext()) {
            Object entry =  iterator3.next();
            //System.out.println(next.getClass());//HashMap$Node -实现-> Map.Entry (getKey,getValue)
            //向下转型 Map.Entry
            Map.Entry m = (Map.Entry) entry;
            System.out.println(m.getKey() + "-" + m.getValue());
        }
    }
}
```

#### 10.3.1 HashMap

![image-20220308130213946](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308130213946.png)

![image-20220308131304588](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308131304588.png)

```java
package com.hspedu.map_;

import java.util.HashMap;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class HashMapSource1 {
    public static void main(String[] args) {
        HashMap map = new HashMap();
        map.put("java", 10);//ok
        map.put("php", 10);//ok
        map.put("java", 20);//替换value

        System.out.println("map=" + map);//

        /*老韩解读HashMap的源码+图解
        1. 执行构造器 new HashMap()
           初始化加载因子 loadfactor = 0.75
           HashMap$Node[] table = null
        2. 执行put 调用 hash方法，计算 key的 hash值 (h = key.hashCode()) ^ (h >>> 16)
            public V put(K key, V value) {//K = "java" value = 10
                return putVal(hash(key), key, value, false, true);
            }
         3. 执行 putVal
         final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
                Node<K,V>[] tab; Node<K,V> p; int n, i;//辅助变量
                //如果底层的table 数组为null, 或者 length =0 , 就扩容到16
                if ((tab = table) == null || (n = tab.length) == 0)
                    n = (tab = resize()).length;
                //取出hash值对应的table的索引位置的Node, 如果为null, 就直接把加入的k-v
                //, 创建成一个 Node ,加入该位置即可
                if ((p = tab[i = (n - 1) & hash]) == null)
                    tab[i] = newNode(hash, key, value, null);
                else {
                    Node<K,V> e; K k;//辅助变量
                // 如果table的索引位置的key的hash相同和新的key的hash值相同，
                 // 并 满足(table现有的结点的key和准备添加的key是同一个对象  || equals返回真)
                 // 就认为不能加入新的k-v
                    if (p.hash == hash &&
                        ((k = p.key) == key || (key != null && key.equals(k))))
                        e = p;
                    else if (p instanceof TreeNode)//如果当前的table的已有的Node 是红黑树，就按照红黑树的方式处理
                        e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
                    else {
                        //如果找到的结点，后面是链表，就循环比较
                        for (int binCount = 0; ; ++binCount) {//死循环
                            if ((e = p.next) == null) {//如果整个链表，没有和他相同,就加到该链表的最后
                                p.next = newNode(hash, key, value, null);
                                //加入后，判断当前链表的个数，是否已经到8个，到8个，后
                                //就调用 treeifyBin 方法进行红黑树的转换
                                if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                                    treeifyBin(tab, hash);
                                break;
                            }
                            if (e.hash == hash && //如果在循环比较过程中，发现有相同,就break,就只是替换value
                                ((k = e.key) == key || (key != null && key.equals(k))))
                                break;
                            p = e;
                        }
                    }
                    if (e != null) { // existing mapping for key
                        V oldValue = e.value;
                        if (!onlyIfAbsent || oldValue == null)
                            e.value = value; //替换，key对应value
                        afterNodeAccess(e);
                        return oldValue;
                    }
                }
                ++modCount;//每增加一个Node ,就size++
                if (++size > threshold[12-24-48])//如size > 临界值，就扩容
                    resize();
                afterNodeInsertion(evict);
                return null;
            }

              5. 关于树化(转成红黑树)
              //如果table 为null ,或者大小还没有到 64，暂时不树化，而是进行扩容.
              //否则才会真正的树化 -> 剪枝
              final void treeifyBin(Node<K,V>[] tab, int hash) {
                int n, index; Node<K,V> e;
                if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
                    resize();

            }
         */


    }
}
```

#### 10.3.2 HashTable

![image-20220308210159379](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308210159379.png)

![image-20220308211328839](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308211328839.png)

```java
package com.hspedu.map_;

import java.util.Hashtable;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class HashTableExercise {
    public static void main(String[] args) {
        Hashtable table = new Hashtable();//ok
        table.put("john", 100); //ok
        //table.put(null, 100); //异常 NullPointerException
        //table.put("john", null);//异常 NullPointerException
        table.put("lucy", 100);//ok
        table.put("lic", 100);//ok
        table.put("lic", 88);//替换
        table.put("hello1", 1);
        table.put("hello2", 1);
        table.put("hello3", 1);
        table.put("hello4", 1);
        table.put("hello5", 1);
        table.put("hello6", 1);
        System.out.println(table);

        //简单说明一下Hashtable的底层
        //1. 底层有数组 Hashtable$Entry[] 初始化大小为 11
        //2. 临界值 threshold 8 = 11 * 0.75
        //3. 扩容: 按照自己的扩容机制来进行即可.
        //4. 执行 方法 addEntry(hash, key, value, index); 添加K-V 封装到Entry
        //5. 当 if (count >= threshold) 满足时，就进行扩容
        //5. 按照 int newCapacity = (oldCapacity << 1) + 1; 的大小扩容.

    }
}
```

#### 10.3.3 Properties

![image-20220308211809010](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308211809010.png)

```java
package com.hspedu.map_;

import java.util.Properties;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class Properties_ {
    public static void main(String[] args) {

        //老韩解读
        //1. Properties 继承  Hashtable
        //2. 可以通过 k-v 存放数据，当然key 和 value 不能为 null
        //增加
        Properties properties = new Properties();
        //properties.put(null, "abc");//抛出 空指针异常
        //properties.put("abc", null); //抛出 空指针异常
        properties.put("john", 100);//k-v
        properties.put("lucy", 100);
        properties.put("lic", 100);
        properties.put("lic", 88);//如果有相同的key ， value被替换

        System.out.println("properties=" + properties);

        //通过k 获取对应值
        System.out.println(properties.get("lic"));//88
        //要求key为String
        System.out.println(properties.getProperty("lic"));//88

        //删除
        properties.remove("lic");
        System.out.println("properties=" + properties);

        //修改
        properties.put("john", "约翰");
        System.out.println("properties=" + properties);
    }
}
```

#### 10.3.4 TreeMap

```java
package com.hspedu.map_;

import java.util.Comparator;
import java.util.TreeMap;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class TreeMap_ {
    public static void main(String[] args) {

        //使用默认的构造器，创建TreeMap, 是无序的(也没有排序)
        /*
            老韩要求：按照传入的 k(String) 的大小进行排序
         */
//        TreeMap treeMap = new TreeMap();
        TreeMap treeMap = new TreeMap(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                //按照传入的 k(String) 的大小进行排序
                //按照K(String) 的长度大小排序
                //return ((String) o2).compareTo((String) o1);
                return ((String) o2).length() - ((String) o1).length();
            }
        });
        treeMap.put("jack", "杰克");
        treeMap.put("tom", "汤姆");
        treeMap.put("kristina", "克瑞斯提诺");
        treeMap.put("smith", "斯密斯");
        treeMap.put("hsp", "韩顺平");//加入不了

        System.out.println("treemap=" + treeMap);

        /*

            老韩解读源码：
            1. 构造器. 把传入的实现了 Comparator接口的匿名内部类(对象)，传给给TreeMap的comparator
             public TreeMap(Comparator<? super K> comparator) {
                this.comparator = comparator;
            }
            2. 调用put方法
            2.1 第一次添加, 把k-v 封装到 Entry对象，放入root
            Entry<K,V> t = root;
            if (t == null) {
                compare(key, key); // type (and possibly null) check

                root = new Entry<>(key, value, null);
                size = 1;
                modCount++;
                return null;
            }
            2.2 以后添加
            Comparator<? super K> cpr = comparator;
            if (cpr != null) {
                do { //遍历所有的key , 给当前key找到适当位置
                    parent = t;
                    cmp = cpr.compare(key, t.key);//动态绑定到我们的匿名内部类的compare
                    if (cmp < 0)
                        t = t.left;
                    else if (cmp > 0)
                        t = t.right;
                    else  //如果遍历过程中，发现准备添加Key 和当前已有的Key 相等，就不添加
                        return t.setValue(value);
                } while (t != null);
            }
         */
    }
}
```



### 10.4 ==开发中如何选择集合类==

![image-20220308224416479](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308224416479.png)

### 10.5 Collections工具类

![image-20220308234134823](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308234134823.png)

![image-20220308234235750](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220308234235750.png)

```java
package com.hspedu.collections_;

import java.util.*;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class Collections_ {
    public static void main(String[] args) {

        //创建ArrayList 集合，用于测试.
        List list = new ArrayList();
        list.add("tom");
        list.add("smith");
        list.add("king");
        list.add("milan");
        list.add("tom");


//        reverse(List)：反转 List 中元素的顺序
        Collections.reverse(list);
        System.out.println("list=" + list);
//        shuffle(List)：对 List 集合元素进行随机排序
//        for (int i = 0; i < 5; i++) {
//            Collections.shuffle(list);
//            System.out.println("list=" + list);
//        }

//        sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
        Collections.sort(list);
        System.out.println("自然排序后");
        System.out.println("list=" + list);
//        sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
        //我们希望按照 字符串的长度大小排序
        Collections.sort(list, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                //可以加入校验代码.
                return ((String) o2).length() - ((String) o1).length();
            }
        });
        System.out.println("字符串长度大小排序=" + list);
//        swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换

        //比如
        Collections.swap(list, 0, 1);
        System.out.println("交换后的情况");
        System.out.println("list=" + list);

        //Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
        System.out.println("自然顺序最大元素=" + Collections.max(list));
        //Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
        //比如，我们要返回长度最大的元素
        Object maxObject = Collections.max(list, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                return ((String)o1).length() - ((String)o2).length();
            }
        });
        System.out.println("长度最大的元素=" + maxObject);


        //Object min(Collection)
        //Object min(Collection，Comparator)
        //上面的两个方法，参考max即可

        //int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
        System.out.println("tom出现的次数=" + Collections.frequency(list, "tom"));

        //void copy(List dest,List src)：将src中的内容复制到dest中

        ArrayList dest = new ArrayList();
        //为了完成一个完整拷贝，我们需要先给dest 赋值，大小和list.size()一样
        for(int i = 0; i < list.size(); i++) {
            dest.add("");
        }
        //拷贝
        Collections.copy(dest, list);
        System.out.println("dest=" + dest);

        //boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值
        //如果list中，有tom 就替换成 汤姆
        Collections.replaceAll(list, "tom", "汤姆");
        System.out.println("list替换后=" + list);

        
    }
}
```

## 11. 泛型

![image-20220309001744524](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220309001744524.png)

```java
package com.hspedu.generic;

import java.util.List;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class Generic03 {
    public static void main(String[] args) {

        //注意，特别强调： E具体的数据类型在定义Person对象的时候指定,即在编译期间，就确定E是什么类型
        Person<String> person = new Person<String>("韩顺平教育");
        person.show(); //String

        /*
            你可以这样理解，上面的Person类
            class Person {
                String s ;//E表示 s的数据类型, 该数据类型在定义Person对象的时候指定,即在编译期间，就确定E是什么类型

                public Person(String s) {//E也可以是参数类型
                    this.s = s;
                }

                public String f() {//返回类型使用E
                    return s;
                }
            }
         */

        Person<Integer> person2 = new Person<Integer>(100);
        person2.show();//Integer

        /*
            class Person {
                Integer s ;//E表示 s的数据类型, 该数据类型在定义Person对象的时候指定,即在编译期间，就确定E是什么类型

                public Person(Integer s) {//E也可以是参数类型
                    this.s = s;
                }

                public Integer f() {//返回类型使用E
                    return s;
                }
            }
         */
    }
}

//泛型的作用是：可以在类声明时通过一个标识表示类中某个属性的类型，
// 或者是某个方法的返回值的类型，或者是参数类型

class Person<E> {
    E s ;//E表示 s的数据类型, 该数据类型在定义Person对象的时候指定,即在编译期间，就确定E是什么类型

    public Person(E s) {//E也可以是参数类型
        this.s = s;
    }

    public E f() {//返回类型使用E
        return s;
    }

    public void show() {
        System.out.println(s.getClass());//显示s的运行类型
    }
}
```

![image-20220309122605099](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220309122605099.png)

![image-20220309124314279](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220309124314279.png)

### 10.1 自定义泛型类

![image-20220310111313873](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310111313873.png)

```java
package com.hspedu.customgeneric;


import java.util.Arrays;

/**
 * @author 韩顺平
 * @version 1.0
 */
@SuppressWarnings({"all"})
public class CustomGeneric_ {
    public static void main(String[] args) {

        //T=Double, R=String, M=Integer
        Tiger<Double,String,Integer> g = new Tiger<>("john");
        g.setT(10.9); //OK
        //g.setT("yy"); //错误，类型不对
        System.out.println(g);
        Tiger g2 = new Tiger("john~~");//OK T=Object R=Object M=Object
        g2.setT("yy"); //OK ,因为 T=Object "yy"=String 是Object子类
        System.out.println("g2=" + g2);

    }
}

//老韩解读
//1. Tiger 后面泛型，所以我们把 Tiger 就称为自定义泛型类
//2, T, R, M 泛型的标识符, 一般是单个大写字母
//3. 泛型标识符可以有多个.
//4. 普通成员可以使用泛型 (属性、方法)
//5. 使用泛型的数组，不能初始化
//6. 静态方法中不能使用类的泛型
class Tiger<T, R, M> {
    String name;
    R r; //属性使用到泛型
    M m;
    T t;
    //因为数组在new 不能确定T的类型，就无法在内存开空间
    T[] ts;

    public Tiger(String name) {
        this.name = name;
    }

    public Tiger(R r, M m, T t) {//构造器使用泛型

        this.r = r;
        this.m = m;
        this.t = t;
    }

    public Tiger(String name, R r, M m, T t) {//构造器使用泛型
        this.name = name;
        this.r = r;
        this.m = m;
        this.t = t;
    }

    //因为静态是和类相关的，在类加载时，对象还没有创建
    //所以，如果静态方法和静态属性使用了泛型，JVM就无法完成初始化
//    static R r2;
//    public static void m1(M m) {
//
//    }

    //方法使用泛型

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public R getR() {
        return r;
    }

    public void setR(R r) {//方法使用到泛型
        this.r = r;
    }

    public M getM() {//返回类型可以使用泛型.
        return m;
    }

    public void setM(M m) {
        this.m = m;
    }

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }

    @Override
    public String toString() {
        return "Tiger{" +
                "name='" + name + '\'' +
                ", r=" + r +
                ", m=" + m +
                ", t=" + t +
                ", ts=" + Arrays.toString(ts) +
                '}';
    }
}

```

### 10.2 自定义泛型接口

![image-20220310112551614](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310112551614.png)

```java
package com.hspedu.customgeneric;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class CustomInterfaceGeneric {
    public static void main(String[] args) {

    }
}

/**
 *  泛型接口使用的说明
 *  1. 接口中，静态成员也不能使用泛型
 *  2. 泛型接口的类型, 在继承接口或者实现接口时确定
 *  3. 没有指定类型，默认为Object
 */

//在继承接口 指定泛型接口的类型
interface IA extends IUsb<String, Double> {

}
//当我们去实现IA接口时，因为IA在继承IUsu 接口时，指定了U 为String R为Double
//，在实现IUsu接口的方法时，使用String替换U, 是Double替换R
class AA implements IA {

    @Override
    public Double get(String s) {
        return null;
    }
    @Override
    public void hi(Double aDouble) {

    }
    @Override
    public void run(Double r1, Double r2, String u1, String u2) {

    }
}

//实现接口时，直接指定泛型接口的类型
//给U 指定Integer 给 R 指定了 Float
//所以，当我们实现IUsb方法时，会使用Integer替换U, 使用Float替换R
class BB implements IUsb<Integer, Float> {

    @Override
    public Float get(Integer integer) {
        return null;
    }

    @Override
    public void hi(Float aFloat) {

    }

    @Override
    public void run(Float r1, Float r2, Integer u1, Integer u2) {

    }
}
//没有指定类型，默认为Object
//建议直接写成 IUsb<Object,Object>
class CC implements IUsb { //等价 class CC implements IUsb<Object,Object> {
    @Override
    public Object get(Object o) {
        return null;
    }
    @Override
    public void hi(Object o) {
    }
    @Override
    public void run(Object r1, Object r2, Object u1, Object u2) {

    }

}

interface IUsb<U, R> {

    int n = 10;
    //U name; 不能这样使用

    //普通方法中，可以使用接口泛型
    R get(U u);

    void hi(R r);

    void run(R r1, R r2, U u1, U u2);

    //在jdk8 中，可以在接口中，使用默认方法, 也是可以使用泛型
    default R method(U u) {
        return null;
    }
}
```

### 10.3 自定义泛型方法

![image-20220310191650751](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310191650751.png)

```java
package com.hspedu.customgeneric;

import java.util.ArrayList;

/**
 * @author 韩顺平
 * @version 1.0
 * 泛型方法的使用
 */
@SuppressWarnings({"all"})
public class CustomMethodGeneric {
    public static void main(String[] args) {
        Car car = new Car();
        car.fly("宝马", 100);//当调用方法时，传入参数，编译器，就会确定类型
        System.out.println("=======");
        car.fly(300, 100.1);//当调用方法时，传入参数，编译器，就会确定类型

        //测试
        //T->String, R-> ArrayList
        Fish<String, ArrayList> fish = new Fish<>();
        fish.hello(new ArrayList(), 11.3f);
    }
}

//泛型方法，可以定义在普通类中, 也可以定义在泛型类中
class Car {//普通类

    public void run() {//普通方法
    }
    //说明 泛型方法
    //1. <T,R> 就是泛型
    //2. 是提供给 fly使用的
    public <T, R> void fly(T t, R r) {//泛型方法
        System.out.println(t.getClass());//String
        System.out.println(r.getClass());//Integer
    }
}

class Fish<T, R> {//泛型类
    public void run() {//普通方法
    }
    public<U,M> void eat(U u, M m) {//泛型方法

    }
    //说明
    //1. 下面hi方法不是泛型方法
    //2. 是hi方法使用了类声明的 泛型
    public void hi(T t) {
    }
    //泛型方法，可以使用类声明的泛型，也可以使用自己声明泛型
    public<K> void hello(R r, K k) {
        System.out.println(r.getClass());//ArrayList
        System.out.println(k.getClass());//Float
    }

}
```

### 10.4 泛型的继承和通配符

![image-20220310192550207](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310192550207.png)

```java
package com.hspedu;

import java.util.ArrayList;
import java.util.List;

/**
 * @author 韩顺平
 * @version 1.0
 * 泛型的继承和通配符
 */
public class GenericExtends {
    public static void main(String[] args) {

        Object o = new String("xx");

        //泛型没有继承性
        //List<Object> list = new ArrayList<String>();

        //举例说明下面三个方法的使用
        List<Object> list1 = new ArrayList<>();
        List<String> list2 = new ArrayList<>();
        List<AA> list3 = new ArrayList<>();
        List<BB> list4 = new ArrayList<>();
        List<CC> list5 = new ArrayList<>();

        //如果是 List<?> c ，可以接受任意的泛型类型
        printCollection1(list1);
        printCollection1(list2);
        printCollection1(list3);
        printCollection1(list4);
        printCollection1(list5);

        //List<? extends AA> c： 表示 上限，可以接受 AA或者AA子类
//        printCollection2(list1);//×
//        printCollection2(list2);//×
        printCollection2(list3);//√
        printCollection2(list4);//√
        printCollection2(list5);//√

        //List<? super AA> c: 支持AA类以及AA类的父类，不限于直接父类
        printCollection3(list1);//√
        //printCollection3(list2);//×
        printCollection3(list3);//√
        //printCollection3(list4);//×
        //printCollection3(list5);//×


        //冒泡排序

        //插入排序

        //....


    }
    // ? extends AA 表示 上限，可以接受 AA或者AA子类
    public static void printCollection2(List<? extends AA> c) {
        for (Object object : c) {
            System.out.println(object);
        }
    }

    //说明: List<?> 表示 任意的泛型类型都可以接受
    public static void printCollection1(List<?> c) {
        for (Object object : c) { // 通配符，取出时，就是Object
            System.out.println(object);
        }
    }



    // ? super 子类类名AA:支持AA类以及AA类的父类，不限于直接父类，
    //规定了泛型的下限
    public static void printCollection3(List<? super AA> c) {
        for (Object object : c) {
            System.out.println(object);
        }
    }

}

class AA {
}

class BB extends AA {
}

class CC extends BB {
}
```

## 12. GUI

### 12.1 原理

![image-20220310204132912](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310204132912.png)

### 12.2 绘图方法

![image-20220310210744699](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310210744699.png)

```java
package com.hspedu.draw;

import javax.swing.*;
import java.awt.*;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示如何在面板上画出圆形
 */
@SuppressWarnings({"all"})
public class DrawCircle extends JFrame { //JFrame对应窗口,可以理解成是一个画框

    //定义一个面板
    private MyPanel mp = null;

    public static void main(String[] args) {
        new DrawCircle();
        System.out.println("退出程序~");
    }

    public DrawCircle() {//构造器
        //初始化面板
        mp = new MyPanel();
        //把面板放入到窗口(画框)
        this.add(mp);
        //设置窗口的大小
        this.setSize(400, 300);
        //当点击窗口的小×，程序完全退出.
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);//可以显示
    }
}

//1.先定义一个MyPanel, 继承JPanel类， 画图形，就在面板上画
class MyPanel extends JPanel {


    //说明:
    //1. MyPanel 对象就是一个画板
    //2. Graphics g 把 g 理解成一支画笔
    //3. Graphics 提供了很多绘图的方法
    //Graphics g
    @Override
    public void paint(Graphics g) {//绘图方法
        super.paint(g);//调用父类的方法完成初始化.
        System.out.println("paint 方法被调用了~");
        //画出一个圆形.
        //g.drawOval(10, 10, 100, 100);


        //演示绘制不同的图形..
        //画直线 drawLine(int x1,int y1,int x2,int y2)
        //g.drawLine(10, 10, 100, 100);
        //画矩形边框 drawRect(int x, int y, int width, int height)
        //g.drawRect(10, 10, 100, 100);
        //画椭圆边框 drawOval(int x, int y, int width, int height)
        //填充矩形 fillRect(int x, int y, int width, int height)
        //设置画笔的颜色
//        g.setColor(Color.blue);
//        g.fillRect(10, 10, 100, 100);

        //填充椭圆 fillOval(int x, int y, int width, int height)
//        g.setColor(Color.red);
//        g.fillOval(10, 10, 100, 100);

        //画图片 drawImage(Image img, int x, int y, ..)
        //1. 获取图片资源, /bg.png 表示在该项目的根目录去获取 bg.png 图片资源
//        Image image = Toolkit.getDefaultToolkit().getImage(Panel.class.getResource("/bg.png"));
//        g.drawImage(image, 10, 10, 175, 221, this);
        //画字符串 drawString(String str, int x, int y)//写字
        //给画笔设置颜色和字体
        g.setColor(Color.red);
        g.setFont(new Font("隶书", Font.BOLD, 50));
        //这里设置的 100， 100， 是 "北京你好"左下角
        g.drawString("北京你好", 100, 100);
        //设置画笔的字体 setFont(Font font)
        //设置画笔的颜色 setColor(Color c)
    }
}
```

### 12.3 键盘监听事件

```java
package com.hspedu.event_;

import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.awt.event.MouseListener;
import java.awt.event.WindowListener;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示小球通过键盘控制上下左右的移动-> 讲解Java的事件控制
 */
public class BallMove extends JFrame { //窗口
    MyPanel mp = null;
    public static void main(String[] args) {
        BallMove ballMove = new BallMove();
    }

    //构造器
    public BallMove() {
        mp = new MyPanel();
        this.add(mp);
        this.setSize(400, 300);
        //窗口JFrame 对象可以监听键盘事件, 即可以监听到面板发生的键盘事件
        this.addKeyListener(mp);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);
    }
}

//面板, 可以画出小球
//KeyListener 是监听器, 可以监听键盘事件
class MyPanel extends JPanel implements KeyListener {

    //为了让小球可以移动, 把他的左上角的坐标(x,y)设置变量
    int x = 10;
    int y = 10;
    @Override
    public void paint(Graphics g) {
        super.paint(g);
        g.fillOval(x, y, 20, 20); //默认黑色
    }

    //有字符输出时，该方法就会触发
    @Override
    public void keyTyped(KeyEvent e) {
    }
    //当某个键按下，该方法会触发
    @Override
    public void keyPressed(KeyEvent e) {

        //System.out.println((char)e.getKeyCode() + "被按下..");

        //根据用户按下的不同键，来处理小球的移动 (上下左右的键)
        //在java中，会给每一个键，分配一个值(int)
        if(e.getKeyCode() == KeyEvent.VK_DOWN) {//KeyEvent.VK_DOWN就是向下的箭头对应的code
            y++;
        } else if(e.getKeyCode() == KeyEvent.VK_UP) {
            y--;
        } else if(e.getKeyCode() == KeyEvent.VK_LEFT) {
            x--;
        } else if(e.getKeyCode() == KeyEvent.VK_RIGHT) {
            x++;
        }
        //让面板重绘
        this.repaint();
    }
    //当某个键释放(松开)，该方法会触发
    @Override
    public void keyReleased(KeyEvent e) {
    }
}
```

### 12.4 java事件处理机制

![image-20220310234542728](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310234542728.png)

![image-20220310234813637](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310234813637.png)

![image-20220310234908676](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310234908676.png)

![image-20220310235105753](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220310235105753.png)

## 13. 多线程

### 13.1 基础

![image-20220311003148636](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311003148636.png)

![image-20220311003122959](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311003122959.png)

![image-20220311114637676](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311114637676.png)

#### 为什么是start方法

```java
package com.hspedu.threaduse;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示通过继承Thread 类创建线程
 */
public class Thread01 {
    public static void main(String[] args) throws InterruptedException {

        //创建Cat对象，可以当做线程使用
        Cat cat = new Cat();

        //老韩读源码
        /*
            (1)
            public synchronized void start() {
                start0();
            }
            (2)
            //start0() 是本地方法，是JVM调用, 底层是c/c++实现
            //真正实现多线程的效果， 是start0(), 而不是 run
            private native void start0();

         */

        cat.start();//启动线程-> 最终会执行cat的run方法



        //cat.run();//run方法就是一个普通的方法, 没有真正的启动一个线程，就会把run方法执行完毕，才向下执行
        //说明: 当main线程启动一个子线程 Thread-0, 主线程不会阻塞, 会继续执行
        //这时 主线程和子线程是交替执行..
        System.out.println("主线程继续执行" + Thread.currentThread().getName());//名字main
        for(int i = 0; i < 60; i++) {
            System.out.println("主线程 i=" + i);
            //让主线程休眠
            Thread.sleep(1000);
        }

    }
}

//老韩说明
//1. 当一个类继承了 Thread 类， 该类就可以当做线程使用
//2. 我们会重写 run方法，写上自己的业务代码
//3. run Thread 类 实现了 Runnable 接口的run方法
/*
    @Override
    public void run() {
        if (target != null) {
            target.run();
        }
    }
 */



class Cat extends Thread {

    int times = 0;
    @Override
    public void run() {//重写run方法，写上自己的业务逻辑

        while (true) {
            //该线程每隔1秒。在控制台输出 “喵喵, 我是小猫咪”
            System.out.println("喵喵, 我是小猫咪" + (++times) + " 线程名=" + Thread.currentThread().getName());
            //让该线程休眠1秒 ctrl+alt+t
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            if(times == 80) {
                break;//当times 到80, 退出while, 这时线程也就退出..
            }
        }
    }
}
```

![image-20220311113345108](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311113345108.png)

### 13.2 线程的基本使用

![image-20220311113428284](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311113428284.png)

#### 代理模式

```java
package com.hspedu.threaduse;

/**
 * @author 韩顺平
 * @version 1.0
 * 通过实现接口Runnable 来开发线程
 */
public class Thread02 {
    public static void main(String[] args) {
        Dog dog = new Dog();
        //dog.start(); 这里不能调用start
        //创建了Thread对象，把 dog对象(实现Runnable),放入Thread
        Thread thread = new Thread(dog);
        thread.start();

//        Tiger tiger = new Tiger();//实现了 Runnable
//        ThreadProxy threadProxy = new ThreadProxy(tiger);
//        threadProxy.start();
    }
}

class Animal {
}

class Tiger extends Animal implements Runnable {

    @Override
    public void run() {
        System.out.println("老虎嗷嗷叫....");
    }
}

//线程代理类 , 模拟了一个极简的Thread类
class ThreadProxy implements Runnable {//你可以把Proxy类当做 ThreadProxy

    private Runnable target = null;//属性，类型是 Runnable

    @Override
    public void run() {
        if (target != null) {
            target.run();//动态绑定（运行类型Tiger）
        }
    }

    public ThreadProxy(Runnable target) {
        this.target = target;
    }

    public void start() {
        start0();//这个方法时真正实现多线程方法
    }

    public void start0() {
        run();
    }
}


class Dog implements Runnable { //通过实现Runnable接口，开发线程

    int count = 0;

    @Override
    public void run() { //普通方法
        while (true) {
            System.out.println("小狗汪汪叫..hi" + (++count) + Thread.currentThread().getName());

            //休眠1秒
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            if (count == 10) {
                break;
            }
        }
    }
}
```

### 13.3 线程终止

![image-20220311115302088](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311115302088.png)

```java
package com.hspedu.exit_;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class ThreadExit_ {
    public static void main(String[] args) throws InterruptedException {
        T t1 = new T();
        t1.start();

        //如果希望main线程去控制t1 线程的终止, 必须可以修改 loop
        //让t1 退出run方法，从而终止 t1线程 -> 通知方式

        //让主线程休眠 10 秒，再通知 t1线程退出
        System.out.println("main线程休眠10s...");
        Thread.sleep(10 * 1000);
        t1.setLoop(false);
    }
}

class T extends Thread {
    private int count = 0;
    //设置一个控制变量
    private boolean loop = true;
    @Override
    public void run() {
        while (loop) {

            try {
                Thread.sleep(50);// 让当前线程休眠50ms
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("T 运行中...." + (++count));
        }

    }

    public void setLoop(boolean loop) {
        this.loop = loop;
    }
}
```

### 13.4 线程常用方法

![image-20220311115857361](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311115857361.png)

![image-20220311120731440](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311120731440.png)

```java
package com.hspedu.method;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class ThreadMethod01 {
    public static void main(String[] args) throws InterruptedException {
        //测试相关的方法
        T t = new T();
        t.setName("老韩");
        t.setPriority(Thread.MIN_PRIORITY);//1
        t.start();//启动子线程


        //主线程打印5 hi ,然后我就中断 子线程的休眠
        for(int i = 0; i < 5; i++) {
            Thread.sleep(1000);
            System.out.println("hi " + i);
        }

        System.out.println(t.getName() + " 线程的优先级 =" + t.getPriority());//1

        t.interrupt();//当执行到这里，就会中断 t线程的休眠.



    }
}

class T extends Thread { //自定义的线程类
    @Override
    public void run() {
        while (true) {
            for (int i = 0; i < 100; i++) {
                //Thread.currentThread().getName() 获取当前线程的名称
                System.out.println(Thread.currentThread().getName() + "  吃包子~~~~" + i);
            }
            try {
                System.out.println(Thread.currentThread().getName() + " 休眠中~~~");
                Thread.sleep(20000);//20秒
            } catch (InterruptedException e) {
                //当该线程执行到一个interrupt 方法时，就会catch 一个 异常, 可以加入自己的业务代码
                //InterruptedException 是捕获到一个中断异常.
                System.out.println(Thread.currentThread().getName() + "被 interrupt了");
            }
        }
    }
}

package com.hspedu.method;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class ThreadMethod02 {
    public static void main(String[] args) throws InterruptedException {

        T2 t2 = new T2();
        t2.start();

        for(int i = 1; i <= 20; i++) {
            Thread.sleep(1000);
            System.out.println("主线程(小弟) 吃了 " + i  + " 包子");
            if(i == 5) {
                System.out.println("主线程(小弟) 让 子线程(老大) 先吃");
                //join, 线程插队
                //t2.join();// 这里相当于让t2 线程先执行完毕
                Thread.yield();//礼让，不一定成功..
                System.out.println("线程(老大) 吃完了 主线程(小弟) 接着吃..");
            }

        }
    }
}

class T2 extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 20; i++) {
            try {
                Thread.sleep(1000);//休眠1秒
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("子线程(老大) 吃了 " + i +  " 包子");
        }
    }
}
```

![image-20220311144407145](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311144407145.png)

```java
package com.hspedu.method;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class ThreadMethod03 {
    public static void main(String[] args) throws InterruptedException {
        MyDaemonThread myDaemonThread = new MyDaemonThread();
        //如果我们希望当main线程结束后，子线程自动结束
        //,只需将子线程设为守护线程即可
        myDaemonThread.setDaemon(true);
        myDaemonThread.start();

        for( int i = 1; i <= 10; i++) {//main线程
            System.out.println("宝强在辛苦的工作...");
            Thread.sleep(1000);
        }
    }
}

class MyDaemonThread extends Thread {
    public void run() {
        for (; ; ) {//无限循环
            try {
                Thread.sleep(1000);//休眠1000毫秒
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("马蓉和宋喆快乐聊天，哈哈哈~~~");
        }
    }
}


```

### 13.5 线程的生命周期

![image-20220311145745793](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311145745793.png)

![image-20220311150322726](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311150322726.png)

```java
package com.hspedu.state_;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class ThreadState_ {
    public static void main(String[] args) throws InterruptedException {
        T t = new T();
        System.out.println(t.getName() + " 状态 " + t.getState());
        t.start();

        while (Thread.State.TERMINATED != t.getState()) {
            System.out.println(t.getName() + " 状态 " + t.getState());
            Thread.sleep(500);
        }

        System.out.println(t.getName() + " 状态 " + t.getState());

    }
}

class T extends Thread {
    @Override
    public void run() {
        while (true) {
            for (int i = 0; i < 10; i++) {
                System.out.println("hi " + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            break;
        }
    }
}
```

### 13.6 线程同步机制

![image-20220311150823710](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311150823710.png)

![image-20220311150851357](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311150851357.png)

![image-20220311150912572](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311150912572.png)

### 13.7 互斥锁

![image-20220311151855583](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311151855583.png)

![image-20220311160202286](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311160202286.png)

```java
package com.hspedu.syn;

/**
 * @author 韩顺平
 * @version 1.0
 * 使用多线程，模拟三个窗口同时售票100张
 */
public class SellTicket {
    public static void main(String[] args) {

        //测试
//        SellTicket01 sellTicket01 = new SellTicket01();
//        SellTicket01 sellTicket02 = new SellTicket01();
//        SellTicket01 sellTicket03 = new SellTicket01();
//
//        //这里我们会出现超卖..
//        sellTicket01.start();//启动售票线程
//        sellTicket02.start();//启动售票线程
//        sellTicket03.start();//启动售票线程


//        System.out.println("===使用实现接口方式来售票=====");
//        SellTicket02 sellTicket02 = new SellTicket02();
//
//        new Thread(sellTicket02).start();//第1个线程-窗口
//        new Thread(sellTicket02).start();//第2个线程-窗口
//        new Thread(sellTicket02).start();//第3个线程-窗口

        //测试一把
        SellTicket03 sellTicket03 = new SellTicket03();
        new Thread(sellTicket03).start();//第1个线程-窗口
        new Thread(sellTicket03).start();//第2个线程-窗口
        new Thread(sellTicket03).start();//第3个线程-窗口

    }
}


//实现接口方式, 使用synchronized实现线程同步
class SellTicket03 implements Runnable {
    private int ticketNum = 100;//让多个线程共享 ticketNum
    private boolean loop = true;//控制run方法变量
    Object object = new Object();


    //同步方法（静态的）的锁为当前类本身
    //老韩解读
    //1. public synchronized static void m1() {} 锁是加在 SellTicket03.class
    //2. 如果在静态方法中，实现一个同步代码块.
    /*
        synchronized (SellTicket03.class) {
            System.out.println("m2");
        }
     */
    public synchronized static void m1() {

    }
    public static  void m2() {
        synchronized (SellTicket03.class) {
            System.out.println("m2");
        }
    }

    //老韩说明
    //1. public synchronized void sell() {} 就是一个同步方法
    //2. 这时锁在 this对象
    //3. 也可以在代码块上写 synchronize ,同步代码块, 互斥锁还是在this对象
    public /*synchronized*/ void sell() { //同步方法, 在同一时刻， 只能有一个线程来执行sell方法

        synchronized (/*this*/ object) {
            if (ticketNum <= 0) {
                System.out.println("售票结束...");
                loop = false;
                return;
            }

            //休眠50毫秒, 模拟
            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票"
                    + " 剩余票数=" + (--ticketNum));//1 - 0 - -1  - -2
        }
    }

    @Override
    public void run() {
        while (loop) {

            sell();//sell方法是一个同步方法
        }
    }
}

//使用Thread方式
// new SellTicket01().start()
// new SellTicket01().start();
class SellTicket01 extends Thread {

    private static int ticketNum = 100;//让多个线程共享 ticketNum

//    public void m1() {
//        synchronized (this) {
//            System.out.println("hello");
//        }
//    }

    @Override
    public void run() {


        while (true) {

            if (ticketNum <= 0) {
                System.out.println("售票结束...");
                break;
            }

            //休眠50毫秒, 模拟
            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票"
                    + " 剩余票数=" + (--ticketNum));

        }
    }
}


//实现接口方式
class SellTicket02 implements Runnable {
    private int ticketNum = 100;//让多个线程共享 ticketNum

    @Override
    public void run() {
        while (true) {

            if (ticketNum <= 0) {
                System.out.println("售票结束...");
                break;
            }

            //休眠50毫秒, 模拟
            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票"
                    + " 剩余票数=" + (--ticketNum));//1 - 0 - -1  - -2

        }
    }
}
```

### 13.8 线程死锁

![image-20220311161457582](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311161457582.png)

```java
package com.hspedu.syn;

/**
 * @author 韩顺平
 * @version 1.0
 * 模拟线程死锁
 */
public class DeadLock_ {
    public static void main(String[] args) {
        //模拟死锁现象
        DeadLockDemo A = new DeadLockDemo(true);
        A.setName("A线程");
        DeadLockDemo B = new DeadLockDemo(false);
        B.setName("B线程");
        A.start();
        B.start();
    }
}


//线程
class DeadLockDemo extends Thread {
    static Object o1 = new Object();// 保证多线程，共享一个对象,这里使用static
    static Object o2 = new Object();
    boolean flag;

    public DeadLockDemo(boolean flag) {//构造器
        this.flag = flag;
    }

    @Override
    public void run() {

        //下面业务逻辑的分析
        //1. 如果flag 为 T, 线程A 就会先得到/持有 o1 对象锁, 然后尝试去获取 o2 对象锁
        //2. 如果线程A 得不到 o2 对象锁，就会Blocked
        //3. 如果flag 为 F, 线程B 就会先得到/持有 o2 对象锁, 然后尝试去获取 o1 对象锁
        //4. 如果线程B 得不到 o1 对象锁，就会Blocked
        if (flag) {
            synchronized (o1) {//对象互斥锁, 下面就是同步代码
                System.out.println(Thread.currentThread().getName() + " 进入1");
                synchronized (o2) { // 这里获得li对象的监视权
                    System.out.println(Thread.currentThread().getName() + " 进入2");
                }
                
            }
        } else {
            synchronized (o2) {
                System.out.println(Thread.currentThread().getName() + " 进入3");
                synchronized (o1) { // 这里获得li对象的监视权
                    System.out.println(Thread.currentThread().getName() + " 进入4");
                }
            }
        }
    }
}
```

### 13.9 释放锁

![image-20220311163251710](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311163251710.png)

![image-20220311163606955](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220311163606955.png)

## 14. 文件操作

### 14.1 基础

![image-20220312233719774](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220312233719774.png)

![image-20220312233650494](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220312233650494.png)

![image-20220312233807975](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220312233807975.png)

![image-20220312234048812](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220312234048812.png)

![](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220312234459765.png)![image-20220313130318164](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313130318164.png)

![image-20220313130438115](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313130438115.png)

### 14.2 节点流

#### InputStream字节输入流

![image-20220313130754610](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313130754610.png)

![image-20220313130919827](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313130919827.png)

![image-20220313132646779](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313132646779.png)

```java
package com.hspedu.inputstream_;

import org.junit.jupiter.api.Test;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示FileInputStream的使用(字节输入流 文件--> 程序)
 */
public class FileInputStream_ {
    public static void main(String[] args) {

    }

    /**
     * 演示读取文件...
     * 单个字节的读取，效率比较低
     * -> 使用 read(byte[] b)
     */
    @Test
    public void readFile01() {
        String filePath = "e:\\hello.txt";
        int readData = 0;
        FileInputStream fileInputStream = null;
        try {
            //创建 FileInputStream 对象，用于读取 文件
            fileInputStream = new FileInputStream(filePath);
            //从该输入流读取一个字节的数据。 如果没有输入可用，此方法将阻止。
            //如果返回-1 , 表示读取完毕
            while ((readData = fileInputStream.read()) != -1) {
                System.out.print((char)readData);//转成char显示
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭文件流，释放资源.
            try {
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

    /**
     * 使用 read(byte[] b) 读取文件，提高效率
     */
    @Test
    public void readFile02() {
        String filePath = "e:\\hello.txt";
        //字节数组
        byte[] buf = new byte[8]; //一次读取8个字节.
        int readLen = 0;
        FileInputStream fileInputStream = null;
        try {
            //创建 FileInputStream 对象，用于读取 文件
            fileInputStream = new FileInputStream(filePath);
            //从该输入流读取最多b.length字节的数据到字节数组。 此方法将阻塞，直到某些输入可用。
            //如果返回-1 , 表示读取完毕
            //如果读取正常, 返回实际读取的字节数
            while ((readLen = fileInputStream.read(buf)) != -1) {
                System.out.print(new String(buf, 0, readLen));//显示
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭文件流，释放资源.
            try {
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

#### OutputStream字节输出流

![image-20220313133054507](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313133054507.png)

```java
package com.hspedu.outputstream_;

import org.junit.jupiter.api.Test;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class FileOutputStream01 {
    public static void main(String[] args) {

    }

    /**
     * 演示使用FileOutputStream 将数据写到文件中,
     * 如果该文件不存在，则创建该文件
     */
    @Test
    public void writeFile() {

        //创建 FileOutputStream对象
        String filePath = "e:\\a.txt";
        FileOutputStream fileOutputStream = null;
        try {
            //得到 FileOutputStream对象 对象
            //老师说明
            //1. new FileOutputStream(filePath) 创建方式，当写入内容是，会覆盖原来的内容
            //2. new FileOutputStream(filePath, true) 创建方式，当写入内容是，是追加到文件后面
            fileOutputStream = new FileOutputStream(filePath, true);
            //写入一个字节
            //fileOutputStream.write('H');//
            //写入字符串
            String str = "hsp,world!";
            //str.getBytes() 可以把 字符串-> 字节数组
            //fileOutputStream.write(str.getBytes());
            /*
            write(byte[] b, int off, int len) 将 len字节从位于偏移量 off的指定字节数组写入此文件输出流
             */
            fileOutputStream.write(str.getBytes(), 0, 3);

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fileOutputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### FileReader字符输入流

![image-20220313135350260](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313135350260.png)

```java
package com.hspedu.reader_;

import org.junit.jupiter.api.Test;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class FileReader_ {
    public static void main(String[] args) {


    }

    /**
     * 单个字符读取文件
     */
    @Test
    public void readFile01() {
        String filePath = "e:\\story.txt";
        FileReader fileReader = null;
        int data = 0;
        //1. 创建FileReader对象
        try {
            fileReader = new FileReader(filePath);
            //循环读取 使用read, 单个字符读取
            while ((data = fileReader.read()) != -1) {
                System.out.print((char) data);
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fileReader != null) {
                    fileReader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 字符数组读取文件
     */
    @Test
    public void readFile02() {
        System.out.println("~~~readFile02 ~~~");
        String filePath = "e:\\story.txt";
        FileReader fileReader = null;

        int readLen = 0;
        char[] buf = new char[8];
        //1. 创建FileReader对象
        try {
            fileReader = new FileReader(filePath);
            //循环读取 使用read(buf), 返回的是实际读取到的字符数
            //如果返回-1, 说明到文件结束
            while ((readLen = fileReader.read(buf)) != -1) {
                System.out.print(new String(buf, 0, readLen));
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fileReader != null) {
                    fileReader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### FileWriter字符输出流

![image-20220313135133419](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313135133419.png)

```java
package com.hspedu.writer_;

import java.io.FileWriter;
import java.io.IOException;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class FileWriter_ {
    public static void main(String[] args) {

        String filePath = "e:\\note.txt";
        //创建FileWriter对象
        FileWriter fileWriter = null;
        char[] chars = {'a', 'b', 'c'};
        try {
            fileWriter = new FileWriter(filePath);//默认是覆盖写入
//            3) write(int):写入单个字符
            fileWriter.write('H');
//            4) write(char[]):写入指定数组
            fileWriter.write(chars);
//            5) write(char[],off,len):写入指定数组的指定部分
            fileWriter.write("韩顺平教育".toCharArray(), 0, 3);
//            6) write（string）：写入整个字符串
            fileWriter.write(" 你好北京~");
            fileWriter.write("风雨之后，定见彩虹");
//            7) write(string,off,len):写入字符串的指定部分
            fileWriter.write("上海天津", 0, 2);
            //在数据量大的情况下，可以使用循环操作.


        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            //对应FileWriter , 一定要关闭流，或者flush才能真正的把数据写入到文件
            //老韩看源码就知道原因.
            /*
                看看代码
                private void writeBytes() throws IOException {
        this.bb.flip();
        int var1 = this.bb.limit();
        int var2 = this.bb.position();

        assert var2 <= var1;

        int var3 = var2 <= var1 ? var1 - var2 : 0;
        if (var3 > 0) {
            if (this.ch != null) {
                assert this.ch.write(this.bb) == var3 : var3;
            } else {
                this.out.write(this.bb.array(), this.bb.arrayOffset() + var2, var3);
            }
        }

        this.bb.clear();
    }
             */
            try {
                //fileWriter.flush();
                //关闭文件流，等价 flush() + 关闭
                fileWriter.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }

        System.out.println("程序结束...");


    }
}
```

### 14.3 处理流

![image-20220313143608238](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313143608238.png)

![image-20220313143719958](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313143719958.png)

![image-20220313144122543](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313144122543.png)

#### BufferedReader与BufferedWriter

![image-20220313153214542](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313153214542.png)

```java
package com.hspedu.reader_;

import java.io.BufferedReader;
import java.io.FileReader;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示bufferedReader 使用
 */
public class BufferedReader_ {
    public static void main(String[] args) throws Exception {

        String filePath = "e:\\a.java";
        //创建bufferedReader
        BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
        //读取
        String line; //按行读取, 效率高
        //说明
        //1. bufferedReader.readLine() 是按行读取文件
        //2. 当返回null 时，表示文件读取完毕
        while ((line = bufferedReader.readLine()) != null) {
            System.out.println(line);
        }

        //关闭流, 这里注意，只需要关闭 BufferedReader ，因为底层会自动的去关闭 节点流
        //FileReader。
        /*
            public void close() throws IOException {
                synchronized (lock) {
                    if (in == null)
                        return;
                    try {
                        in.close();//in 就是我们传入的 new FileReader(filePath), 关闭了.
                    } finally {
                        in = null;
                        cb = null;
                    }
                }
            }

         */
        bufferedReader.close();

    }
}
```

```java
package com.hspedu.writer_;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示BufferedWriter的使用
 */
public class BufferedWriter_ {
    public static void main(String[] args) throws IOException {
        String filePath = "e:\\ok.txt";
        //创建BufferedWriter
        //说明:
        //1. new FileWriter(filePath, true) 表示以追加的方式写入
        //2. new FileWriter(filePath) , 表示以覆盖的方式写入
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath));
        bufferedWriter.write("hello, 韩顺平教育!");
        bufferedWriter.newLine();//插入一个和系统相关的换行
        bufferedWriter.write("hello2, 韩顺平教育!");
        bufferedWriter.newLine();
        bufferedWriter.write("hello3, 韩顺平教育!");
        bufferedWriter.newLine();

        //说明：关闭外层流即可 ， 传入的 new FileWriter(filePath) ,会在底层关闭
        bufferedWriter.close();

    }
}
```

```java
package com.hspedu.writer_;

import java.io.*;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class BufferedCopy_ {

    public static void main(String[] args) {


        //老韩说明
        //1. BufferedReader 和 BufferedWriter 是安装字符操作
        //2. 不要去操作 二进制文件[声音，视频，doc, pdf ], 可能造成文件损坏
        //BufferedInputStream
        //BufferedOutputStream
        String srcFilePath = "e:\\a.java";
        String destFilePath = "e:\\a2.java";
//        String srcFilePath = "e:\\0245_韩顺平零基础学Java_引出this.avi";
//        String destFilePath = "e:\\a2韩顺平.avi";
        BufferedReader br = null;
        BufferedWriter bw = null;
        String line;
        try {
            br = new BufferedReader(new FileReader(srcFilePath));
            bw = new BufferedWriter(new FileWriter(destFilePath));

            //说明: readLine 读取一行内容，但是没有换行
            while ((line = br.readLine()) != null) {
                //每读取一行，就写入
                bw.write(line);
                //插入一个换行
                bw.newLine();
            }
            System.out.println("拷贝完毕...");

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭流
            try {
                if(br != null) {
                    br.close();
                }
                if(bw != null) {
                    bw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### BufferedInputStream与BufferedOutputStream

![image-20220313161354242](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313161354242.png)

![image-20220313161530108](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313161530108.png)

![image-20220313161451227](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313161451227.png)![image-20220313161601164](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313161601164.png)

```java
package com.hspedu.outputstream_;

import java.io.*;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示使用BufferedOutputStream 和 BufferedInputStream使用
 * 使用他们，可以完成二进制文件拷贝.
 * 思考：字节流可以操作二进制文件，可以操作文本文件吗？当然可以
 */
public class BufferedCopy02 {
    public static void main(String[] args) {

//        String srcFilePath = "e:\\Koala.jpg";
//        String destFilePath = "e:\\hsp.jpg";
//        String srcFilePath = "e:\\0245_韩顺平零基础学Java_引出this.avi";
//        String destFilePath = "e:\\hsp.avi";
        String srcFilePath = "e:\\a.java";
        String destFilePath = "e:\\a3.java";

        //创建BufferedOutputStream对象BufferedInputStream对象
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //因为 FileInputStream  是 InputStream 子类
            bis = new BufferedInputStream(new FileInputStream(srcFilePath));
            bos = new BufferedOutputStream(new FileOutputStream(destFilePath));

            //循环的读取文件，并写入到 destFilePath
            byte[] buff = new byte[1024];
            int readLen = 0;
            //当返回 -1 时，就表示文件读取完毕
            while ((readLen = bis.read(buff)) != -1) {
                bos.write(buff, 0, readLen);
            }

            System.out.println("文件拷贝完毕~~~");

        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            //关闭流 , 关闭外层的处理流即可，底层会去关闭节点流
            try {
                if(bis != null) {
                    bis.close();
                }
                if(bos != null) {
                    bos.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

        }


    }
}
```

#### 对象流-ObjectInputStream与ObjectOutputStream

![image-20220313164007446](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313164007446.png)

![image-20220313164229265](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313164229265.png)

![image-20220313164117089](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313164117089.png)

```java
package com.hspedu.inputstream_;



import com.hspedu.outputstream_.Dog;

import java.io.*;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class ObjectInputStream_ {
    public static void main(String[] args) throws IOException, ClassNotFoundException {

        //指定反序列化的文件
        String filePath = "e:\\data.dat";

        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filePath));

        //读取
        //老师解读
        //1. 读取(反序列化)的顺序需要和你保存数据(序列化)的顺序一致
        //2. 否则会出现异常

        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());

        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());


        //dog 的编译类型是 Object , dog 的运行类型是 Dog
        Object dog = ois.readObject();
        System.out.println("运行类型=" + dog.getClass());
        System.out.println("dog信息=" + dog);//底层 Object -> Dog

        //这里是特别重要的细节:

        //1. 如果我们希望调用Dog的方法, 需要向下转型
        //2. 需要我们将Dog类的定义，放在到可以引用的位置
        Dog dog2 = (Dog)dog;
        System.out.println(dog2.getName()); //旺财..

        //关闭流, 关闭外层流即可，底层会关闭 FileInputStream 流
        ois.close();


    }
}
```

![image-20220313164152662](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313164152662.png)

```java
package com.hspedu.outputstream_;

import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示ObjectOutputStream的使用, 完成数据的序列化
 */
public class ObjectOutStream_ {
    public static void main(String[] args) throws Exception {
        //序列化后，保存的文件格式，不是存文本，而是按照他的格式来保存
        String filePath = "e:\\data.dat";

        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));

        //序列化数据到 e:\data.dat
        oos.writeInt(100);// int -> Integer (实现了 Serializable)
        oos.writeBoolean(true);// boolean -> Boolean (实现了 Serializable)
        oos.writeChar('a');// char -> Character (实现了 Serializable)
        oos.writeDouble(9.5);// double -> Double (实现了 Serializable)
        oos.writeUTF("韩顺平教育");//String
        //保存一个dog对象
        oos.writeObject(new Dog("旺财", 10, "日本", "白色"));
        oos.close();
        System.out.println("数据保存完毕(序列化形式)");


    }
}
```

![image-20220313170754387](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313170754387.png)

```java
package com.hspedu.outputstream_;

import java.io.Serializable;

/**
 * @author 韩顺平
 * @version 1.0
 */
//如果需要序列化某个类的对象，实现 Serializable
public class Dog implements Serializable {
    private String name;
    private int age;
    //序列化对象时，默认将里面所有属性都进行序列化，但除了static或transient修饰的成员
    private static String nation;
    private transient String color;
    //序列化对象时，要求里面属性的类型也需要实现序列化接口
    private Master master = new Master();

    //serialVersionUID 序列化的版本号，可以提高兼容性
    private static final long serialVersionUID = 1L;

    public Dog(String name, int age, String nation, String color) {
        this.name = name;
        this.age = age;
        this.color = color;
        this.nation = nation;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", color='" + color + '\'' +
                '}' + nation + " " +master;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

}
```

#### 标准输入与标准输出

![image-20220313175142347](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313175142347.png)

```java
package com.hspedu.standard;

import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Scanner;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class InputAndOutput {
    public static void main(String[] args) {
        //System 类 的 public final static InputStream in = null;
        // System.in 编译类型   InputStream
        // System.in 运行类型   BufferedInputStream
        // 表示的是标准输入 键盘
        System.out.println(System.in.getClass());

        //老韩解读
        //1. System.out public final static PrintStream out = null;
        //2. 编译类型 PrintStream
        //3. 运行类型 PrintStream
        //4. 表示标准输出 显示器
        System.out.println(System.out.getClass());

        System.out.println("hello, 韩顺平教育~");

        Scanner scanner = new Scanner(System.in);
        System.out.println("输入内容");
        String next = scanner.next();
        System.out.println("next=" + next);
    }
}
```

#### 转换流-InputStreamReader与OutputStreamWriter

![image-20220313235300448](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313235300448.png)

![image-20220313235735517](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313235735517.png)

![image-20220313235824308](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220313235824308.png)

```java
package com.hspedu.transformation;

import java.io.*;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示使用 InputStreamReader 转换流解决中文乱码问题
 * 将字节流 FileInputStream 转成字符流  InputStreamReader, 指定编码 gbk/utf-8
 */
public class InputStreamReader_ {
    public static void main(String[] args) throws IOException {

        String filePath = "e:\\a.txt";
        //解读
        //1. 把 FileInputStream 转成 InputStreamReader
        //2. 指定编码 gbk
        //InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath), "gbk");
        //3. 把 InputStreamReader 传入 BufferedReader
        //BufferedReader br = new BufferedReader(isr);

        //将2 和 3 合在一起
        BufferedReader br = new BufferedReader(new InputStreamReader(
                                                    new FileInputStream(filePath), "gbk"));

        //4. 读取
        String s = br.readLine();
        System.out.println("读取内容=" + s);
        //5. 关闭外层流
        br.close();

    }
}
```

```java
package com.hspedu.transformation;

import java.io.*;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示 OutputStreamWriter 使用
 * 把FileOutputStream 字节流，转成字符流 OutputStreamWriter
 * 指定处理的编码 gbk/utf-8/utf8
 */
public class OutputStreamWriter_ {
    public static void main(String[] args) throws IOException {
        String filePath = "e:\\hsp.txt";
        String charSet = "utf-8";
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(filePath), charSet);
        osw.write("hi, 韩顺平教育");
        osw.close();
        System.out.println("按照 " + charSet + " 保存文件成功~");
    }
}
```

#### 打印流-PrintStream与PrintWriter

![image-20220314001624450](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314001624450.png)

![image-20220314001547436](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314001547436.png)

```java
package com.hspedu.printstream;

import java.io.IOException;
import java.io.PrintStream;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示PrintStream （字节打印流/输出流）
 */
public class PrintStream_ {
    public static void main(String[] args) throws IOException {

        PrintStream out = System.out;
        //在默认情况下，PrintStream 输出数据的位置是 标准输出，即显示器
        /*
             public void print(String s) {
                if (s == null) {
                    s = "null";
                }
                write(s);
            }

         */
        out.print("john, hello");
        //因为print底层使用的是write , 所以我们可以直接调用write进行打印/输出
        out.write("韩顺平,你好".getBytes());
        out.close();

        //我们可以去修改打印流输出的位置/设备
        //1. 输出修改成到 "e:\\f1.txt"
        //2. "hello, 韩顺平教育~" 就会输出到 e:\f1.txt
        //3. public static void setOut(PrintStream out) {
        //        checkIO();
        //        setOut0(out); // native 方法，修改了out
        //   }
        System.setOut(new PrintStream("e:\\f1.txt"));
        System.out.println("hello, 韩顺平教育~");
    }
}
```

![image-20220314001557684](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314001557684.png)

```java
package com.hspedu.transformation;

import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示 PrintWriter 使用方式
 */
public class PrintWriter_ {
    public static void main(String[] args) throws IOException {

        //PrintWriter printWriter = new PrintWriter(System.out);
        PrintWriter printWriter = new PrintWriter(new FileWriter("e:\\f2.txt"));
        printWriter.print("hi, 北京你好~~~~");
        printWriter.close();//flush + 关闭流, 才会将数据写入到文件..
    }
```

### 14.4 Properties

![image-20220314010942625](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314010942625.png)

![image-20220314011500304](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314011500304.png)

```java
package com.hspedu.properties_;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class Properties01 {
    public static void main(String[] args) throws IOException {


        //读取mysql.properties 文件，并得到ip, user 和 pwd
        BufferedReader br = new BufferedReader(new FileReader("src\\mysql.properties"));
        String line = "";
        while ((line = br.readLine()) != null) { //循环读取
            String[] split = line.split("=");
            //如果我们要求指定的ip值
            if("ip".equals(split[0])) {
                System.out.println(split[0] + "值是: " + split[1]);
            }
        }

        br.close();
    }
}
```

```java
package com.hspedu.properties_;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.Properties;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class Properties02 {
    public static void main(String[] args) throws IOException {
        //使用Properties 类来读取mysql.properties 文件

        //1. 创建Properties 对象
        Properties properties = new Properties();
        //2. 加载指定配置文件
        properties.load(new FileReader("src\\mysql.properties"));
        //3. 把k-v显示控制台
        properties.list(System.out);
        //4. 根据key 获取对应的值
        String user = properties.getProperty("user");
        String pwd = properties.getProperty("pwd");
        System.out.println("用户名=" + user);
        System.out.println("密码是=" + pwd);



    }
}
```

```java
package com.hspedu.properties_;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

/**
 * @author 韩顺平
 * @version 1.0
 */
public class Properties03 {
    public static void main(String[] args) throws IOException {
        //使用Properties 类来创建 配置文件, 修改配置文件内容

        Properties properties = new Properties();
        //创建
        //1.如果该文件没有key 就是创建
        //2.如果该文件有key ,就是修改
        /*
            Properties 父类是 Hashtable ， 底层就是Hashtable 核心方法
            public synchronized V put(K key, V value) {
                // Make sure the value is not null
                if (value == null) {
                    throw new NullPointerException();
                }

                // Makes sure the key is not already in the hashtable.
                Entry<?,?> tab[] = table;
                int hash = key.hashCode();
                int index = (hash & 0x7FFFFFFF) % tab.length;
                @SuppressWarnings("unchecked")
                Entry<K,V> entry = (Entry<K,V>)tab[index];
                for(; entry != null ; entry = entry.next) {
                    if ((entry.hash == hash) && entry.key.equals(key)) {
                        V old = entry.value;
                        entry.value = value;//如果key 存在，就替换
                        return old;
                    }
                }

                addEntry(hash, key, value, index);//如果是新k, 就addEntry
                return null;
            }

         */
        properties.setProperty("charset", "utf8");
        properties.setProperty("user", "汤姆");//注意保存时，是中文的 unicode码值
        properties.setProperty("pwd", "888888");

        //将k-v 存储文件中即可
        properties.store(new FileOutputStream("src\\mysql2.properties"), null);
        System.out.println("保存配置文件成功~");
    }
}
```

## 15. 网络编程

#### 15.1 网络基本概念

##### 15.1.1 网络通信

![image-20220314223809949](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314223809949.png)

##### 15.1.2 网络

![image-20220314223853735](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314223853735.png) 

##### 15.1.3 IP地址

![image-20220314223943494](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314223943494.png)

![image-20220314224102713](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314224102713.png)

##### 15.1.4 域名

![image-20220314224131980](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314224131980.png)

##### 15.1.5 网络通信协议

![image-20220314224959733](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314224959733.png)

![image-20220314224943534](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314224943534.png)

![image-20220314225812845](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314225812845.png)

#### 15.2 InetAddress类

![image-20220314230909350](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314230909350.png)

```java
package com.hspedu.api;
import java.net.InetAddress;
import java.net.UnknownHostException;

/**
 * @author 韩顺平
 * @version 1.0
 * 演示InetAddress 类的使用
 */
public class API_ {
    public static void main(String[] args) throws UnknownHostException {

        //1. 获取本机的InetAddress 对象
        InetAddress localHost = InetAddress.getLocalHost();
        System.out.println(localHost);//DESKTOP-S4MP84S/192.168.12.1

        //2. 根据指定主机名 获取 InetAddress对象
        InetAddress host1 = InetAddress.getByName("DESKTOP-S4MP84S");
        System.out.println("host1=" + host1);//DESKTOP-S4MP84S/192.168.12.1

        //3. 根据域名返回 InetAddress对象, 比如 www.baidu.com 对应
        InetAddress host2 = InetAddress.getByName("www.baidu.com");
        System.out.println("host2=" + host2);//www.baidu.com / 110.242.68.4

        //4. 通过 InetAddress 对象，获取对应的地址
        String hostAddress = host2.getHostAddress();//IP 110.242.68.4
        System.out.println("host2 对应的ip = " + hostAddress);//110.242.68.4

        //5. 通过 InetAddress 对象，获取对应的主机名/域名
        String hostName = host2.getHostName();
        System.out.println("host2对应的主机名/域名=" + hostName); // www.baidu.com

    }
}
```

#### 15.3 Socket

![image-20220314231347932](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314231347932.png)

![image-20220314231548542](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314231548542.png)

#### 15.4 TCP网络通信编程

![image-20220314231850678](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220314231850678.png)

##### 案例1

![image-20220315122440686](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315122440686.png)

```java
package com.hspedu.socket;

import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @author 韩顺平
 * @version 1.0
 * 服务端
 */
public class SocketTCP01Server {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 在本机 的9999端口监听, 等待连接
        //   细节: 要求在本机没有其它服务在监听9999
        //   细节：这个 ServerSocket 可以通过 accept() 返回多个Socket[多个客户端连接服务器的并发]
        ServerSocket serverSocket = new ServerSocket(9999);
        System.out.println("服务端，在9999端口监听，等待连接..");
        //2. 当没有客户端连接9999端口时，程序会 阻塞, 等待连接
        //   如果有客户端连接，则会返回Socket对象，程序继续

        Socket socket = serverSocket.accept();

        System.out.println("服务端 socket =" + socket.getClass());
        //
        //3. 通过socket.getInputStream() 读取客户端写入到数据通道的数据, 显示
        InputStream inputStream = socket.getInputStream();
        //4. IO读取
        byte[] buf = new byte[1024];
        int readLen = 0;
        while ((readLen = inputStream.read(buf)) != -1) {
            System.out.println(new String(buf, 0, readLen));//根据读取到的实际长度，显示内容.
        }
        //5.关闭流和socket
        inputStream.close();
        socket.close();
        serverSocket.close();//关闭
    }
}
```

```java
package com.hspedu.socket;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
import java.net.UnknownHostException;

/**
 * @author 韩顺平
 * @version 1.0
 * 客户端，发送 "hello, server" 给服务端
 */
public class SocketTCP01Client {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 连接服务端 (ip , 端口）
        //解读: 连接本机的 9999端口, 如果连接成功，返回Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("客户端 socket返回=" + socket.getClass());
        //2. 连接上后，生成Socket, 通过socket.getOutputStream()
        //   得到 和 socket对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
        //3. 通过输出流，写入数据到 数据通道
        outputStream.write("hello, server".getBytes());
        //4. 关闭流对象和socket, 必须关闭
        outputStream.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
}
```

##### 案例2

![image-20220315122527232](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315122527232.png)

```java
package com.hspedu.socket;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @author 韩顺平
 * @version 1.0
 * 服务端
 */
@SuppressWarnings({"all"})
public class SocketTCP02Server {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 在本机 的9999端口监听, 等待连接
        //   细节: 要求在本机没有其它服务在监听9999
        //   细节：这个 ServerSocket 可以通过 accept() 返回多个Socket[多个客户端连接服务器的并发]
        ServerSocket serverSocket = new ServerSocket(9999);
        System.out.println("服务端，在9999端口监听，等待连接..");
        //2. 当没有客户端连接9999端口时，程序会 阻塞, 等待连接
        //   如果有客户端连接，则会返回Socket对象，程序继续

        Socket socket = serverSocket.accept();

        System.out.println("服务端 socket =" + socket.getClass());
        //
        //3. 通过socket.getInputStream() 读取客户端写入到数据通道的数据, 显示
        InputStream inputStream = socket.getInputStream();
        //4. IO读取
        byte[] buf = new byte[1024];
        int readLen = 0;
        while ((readLen = inputStream.read(buf)) != -1) {
            System.out.println(new String(buf, 0, readLen));//根据读取到的实际长度，显示内容.
        }
        //5. 获取socket相关联的输出流
        OutputStream outputStream = socket.getOutputStream();
        outputStream.write("hello, client".getBytes());
        //   设置结束标记
        socket.shutdownOutput();

        //6.关闭流和socket
        outputStream.close();
        inputStream.close();
        socket.close();
        serverSocket.close();//关闭

    }
}
```

```java
package com.hspedu.socket;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

/**
 * @author 韩顺平
 * @version 1.0
 * 客户端，发送 "hello, server" 给服务端
 */
@SuppressWarnings({"all"})
public class SocketTCP02Client {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 连接服务端 (ip , 端口）
        //解读: 连接本机的 9999端口, 如果连接成功，返回Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("客户端 socket返回=" + socket.getClass());
        //2. 连接上后，生成Socket, 通过socket.getOutputStream()
        //   得到 和 socket对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
        //3. 通过输出流，写入数据到 数据通道
        outputStream.write("hello, server".getBytes());
        //   设置结束标记
        socket.shutdownOutput();

        //4. 获取和socket关联的输入流. 读取数据(字节)，并显示
        InputStream inputStream = socket.getInputStream();
        byte[] buf = new byte[1024];
        int readLen = 0;
        while ((readLen = inputStream.read(buf)) != -1) {
            System.out.println(new String(buf, 0, readLen));
        }

        //5. 关闭流对象和socket, 必须关闭
        inputStream.close();
        outputStream.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
}
```

##### 案例3

![image-20220315122808919](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315122808919.png)

```java
package com.hspedu.socket;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @author 韩顺平
 * @version 1.0
 * 服务端, 使用字符流方式读写
 */
@SuppressWarnings({"all"})
public class SocketTCP03Server {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 在本机 的9999端口监听, 等待连接
        //   细节: 要求在本机没有其它服务在监听9999
        //   细节：这个 ServerSocket 可以通过 accept() 返回多个Socket[多个客户端连接服务器的并发]
        ServerSocket serverSocket = new ServerSocket(9999);
        System.out.println("服务端，在9999端口监听，等待连接..");
        //2. 当没有客户端连接9999端口时，程序会 阻塞, 等待连接
        //   如果有客户端连接，则会返回Socket对象，程序继续

        Socket socket = serverSocket.accept();

        System.out.println("服务端 socket =" + socket.getClass());
        //
        //3. 通过socket.getInputStream() 读取客户端写入到数据通道的数据, 显示
        InputStream inputStream = socket.getInputStream();
        //4. IO读取, 使用字符流, 老师使用 InputStreamReader 将 inputStream 转成字符流
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String s = bufferedReader.readLine();
        System.out.println(s);//输出

        //5. 获取socket相关联的输出流
        OutputStream outputStream = socket.getOutputStream();
       //    使用字符输出流的方式回复信息
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
        bufferedWriter.write("hello client 字符流");
        bufferedWriter.newLine();// 插入一个换行符，表示回复内容的结束
        bufferedWriter.flush();//注意需要手动的flush


        //6.关闭流和socket
        bufferedWriter.close();
        bufferedReader.close();
        socket.close();
        serverSocket.close();//关闭

    }
}
```

```java
package com.hspedu.socket;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;

/**
 * @author 韩顺平
 * @version 1.0
 * 客户端，发送 "hello, server" 给服务端， 使用字符流
 */
@SuppressWarnings({"all"})
public class SocketTCP03Client {
    public static void main(String[] args) throws IOException {
        //思路
        //1. 连接服务端 (ip , 端口）
        //解读: 连接本机的 9999端口, 如果连接成功，返回Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("客户端 socket返回=" + socket.getClass());
        //2. 连接上后，生成Socket, 通过socket.getOutputStream()
        //   得到 和 socket对象关联的输出流对象
        OutputStream outputStream = socket.getOutputStream();
        //3. 通过输出流，写入数据到 数据通道, 使用字符流
        BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
        bufferedWriter.write("hello, server 字符流");
        bufferedWriter.newLine();//插入一个换行符，表示写入的内容结束, 注意，要求对方使用readLine()!!!!
        bufferedWriter.flush();// 如果使用的字符流，需要手动刷新，否则数据不会写入数据通道
        //4. 获取和socket关联的输入流. 读取数据(字符)，并显示
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String s = bufferedReader.readLine();
        System.out.println(s);

        //5. 关闭流对象和socket, 必须关闭
        bufferedReader.close();//关闭外层流
        bufferedWriter.close();
        socket.close();
        System.out.println("客户端退出.....");
    }
}
```

##### 案例4（收发文件）

![image-20220315122952638](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315122952638.png)

![image-20220315123016282](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315123016282.png)

```java
package com.hspedu.upload;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @author 韩顺平
 * @version 1.0
 * 文件上传的服务端
 */
public class TCPFileUploadServer {
    public static void main(String[] args) throws Exception {

        //1. 服务端在本机监听8888端口
        ServerSocket serverSocket = new ServerSocket(8888);
        System.out.println("服务端在8888端口监听....");
        //2. 等待连接
        Socket socket = serverSocket.accept();


        //3. 读取客户端发送的数据
        //   通过Socket得到输入流
        BufferedInputStream bis = new BufferedInputStream(socket.getInputStream());
        byte[] bytes = StreamUtils.streamToByteArray(bis);
        //4. 将得到 bytes 数组，写入到指定的路径，就得到一个文件了
        String destFilePath = "src\\abc.mp4";
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFilePath));
        bos.write(bytes);
        bos.close();

        // 向客户端回复 "收到图片"
        // 通过socket 获取到输出流(字符)
        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
        writer.write("收到图片");
        writer.flush();//把内容刷新到数据通道
        socket.shutdownOutput();//设置写入结束标记

        //关闭其他资源
        writer.close();
        bis.close();
        socket.close();
        serverSocket.close();
    }
}
```

```java
package com.hspedu.upload;

import java.io.*;
import java.net.InetAddress;
import java.net.Socket;


/**
 * @author 韩顺平
 * @version 1.0
 * 文件上传的客户端
 */
public class TCPFileUploadClient {
    public static void main(String[] args) throws Exception {

        //客户端连接服务端 8888，得到Socket对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 8888);
        //创建读取磁盘文件的输入流
        //String filePath = "e:\\qie.png";
        String filePath = "e:\\abc.mp4";
        BufferedInputStream bis  = new BufferedInputStream(new FileInputStream(filePath));

        //bytes 就是filePath对应的字节数组
        byte[] bytes = StreamUtils.streamToByteArray(bis);

        //通过socket获取到输出流, 将bytes数据发送给服务端
        BufferedOutputStream bos = new BufferedOutputStream(socket.getOutputStream());
        bos.write(bytes);//将文件对应的字节数组的内容，写入到数据通道
        bis.close();
        socket.shutdownOutput();//设置写入数据的结束标记

        //=====接收从服务端回复的消息=====

        InputStream inputStream = socket.getInputStream();
        //使用StreamUtils 的方法，直接将 inputStream 读取到的内容 转成字符串
        String s = StreamUtils.streamToString(inputStream);
        System.out.println(s);


        //关闭相关的流
        inputStream.close();
        bos.close();
        socket.close();

    }
}
```

```java
package com.hspedu.upload;

import java.io.BufferedReader;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

/**
 * 此类用于演示关于流的读写方法
 *
 */
public class StreamUtils {
	/**
	 * 功能：将输入流转换成byte[]
	 * @param is
	 * @return
	 * @throws Exception
	 */
	public static byte[] streamToByteArray(InputStream is) throws Exception{
		ByteArrayOutputStream bos = new ByteArrayOutputStream();//创建输出流对象
		byte[] b = new byte[1024];
		int len;
		while((len=is.read(b))!=-1){
			bos.write(b, 0, len);	
		}
		byte[] array = bos.toByteArray();
		bos.close();
		return array;
	}
	/**
	 * 功能：将InputStream转换成String
	 * @param is
	 * @return
	 * @throws Exception
	 */
	
	public static String streamToString(InputStream is) throws Exception{
		BufferedReader reader = new BufferedReader(new InputStreamReader(is));
		StringBuilder builder= new StringBuilder();
		String line;
		while((line=reader.readLine())!=null){ //当读取到 null时，就表示结束
			builder.append(line+"\r\n");
		}
		return builder.toString();
		
	}
}
```

##### netstat 指令

![image-20220315123835887](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315123835887.png)

![image-20220315124619842](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315124619842.png)

##### TCP网络通讯的小秘密

![image-20220315125252289](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315125252289.png)

![image-20220315125157244](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315125157244.png)

#### 15.5 UDP网络通信编程（了解）

![image-20220315125908420](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315125908420.png)

![image-20220315130406426](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315130406426.png)

![image-20220315130445553](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315130445553.png)

##### 案例1

![image-20220315130814776](https://cdn.jsdelivr.net/gh/Roozenlz/myimage/image-20220315130814776.png)

```java
package com.hspedu.udp;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

/**
 * @author 韩顺平
 * @version 1.0
 * UDP接收端
 */
public class UDPReceiverA {
    public static void main(String[] args) throws IOException {
        //1. 创建一个 DatagramSocket 对象，准备在9999接收数据
        DatagramSocket socket = new DatagramSocket(9999);
        //2. 构建一个 DatagramPacket 对象，准备接收数据
        //   在前面讲解UDP 协议时，老师说过一个数据包最大 64k
        byte[] buf = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buf, buf.length);
        //3. 调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //老师提示: 当有数据包发送到 本机的9999端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9999端口, 就会阻塞等待.
        System.out.println("接收端A 等待接收数据..");
        socket.receive(packet);

        //4. 可以把packet 进行拆包，取出数据，并显示.
        int length = packet.getLength();//实际接收到的数据字节长度
        byte[] data = packet.getData();//接收到数据
        String s = new String(data, 0, length);
        System.out.println(s);


        //===回复信息给B端
        //将需要发送的数据，封装到 DatagramPacket对象
        data = "好的, 明天见".getBytes();
        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        packet =
                new DatagramPacket(data, data.length, InetAddress.getByName("192.168.12.1"), 9998);

        socket.send(packet);//发送

        //5. 关闭资源
        socket.close();
        System.out.println("A端退出...");

    }
}
```

```java
package com.hspedu.udp;

import java.io.IOException;
import java.net.*;

/**
 * @author 韩顺平
 * @version 1.0
 * 发送端B ====> 也可以接收数据
 */
@SuppressWarnings({"all"})
public class UDPSenderB {
    public static void main(String[] args) throws IOException {

        //1.创建 DatagramSocket 对象，准备在9998端口 接收数据
        DatagramSocket socket = new DatagramSocket(9998);

        //2. 将需要发送的数据，封装到 DatagramPacket对象
        byte[] data = "hello 明天吃火锅~".getBytes(); //

        //说明: 封装的 DatagramPacket对象 data 内容字节数组 , data.length , 主机(IP) , 端口
        DatagramPacket packet =
                new DatagramPacket(data, data.length, InetAddress.getByName("192.168.12.1"), 9999);

        socket.send(packet);

        //3.=== 接收从A端回复的信息
        //(1)   构建一个 DatagramPacket 对象，准备接收数据
        //   在前面讲解UDP 协议时，老师说过一个数据包最大 64k
        byte[] buf = new byte[1024];
        packet = new DatagramPacket(buf, buf.length);
        //(2)    调用 接收方法, 将通过网络传输的 DatagramPacket 对象
        //   填充到 packet对象
        //老师提示: 当有数据包发送到 本机的9998端口时，就会接收到数据
        //   如果没有数据包发送到 本机的9998端口, 就会阻塞等待.
        socket.receive(packet);

        //(3)  可以把packet 进行拆包，取出数据，并显示.
        int length = packet.getLength();//实际接收到的数据字节长度
        data = packet.getData();//接收到数据
        String s = new String(data, 0, length);
        System.out.println(s);

        //关闭资源
        socket.close();
        System.out.println("B端退出");
    }
}
```

## 16.反射

