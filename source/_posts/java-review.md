---
title: java复习笔记
date: 2023-03-22 22:25:24
tags: java
categories: review
excerpt: 整理了一下Java复习的知识
---

# java复习

## UML类图

示例：[UML类图-校园活动管理系统](https://www.processon.com/view/link/633bec4ef346fb07dea52d90)

在关联关系中注意：箭头方向(单向关联/双向关联)，对应数量(1..*)，引用名(单向关联/双向关联)【书本15页】

在继承关系中注意：箭头方向【书本18页】

## 函数

### 形参按值传递

```java
public class Test{
	static void fun(int a) {a++;}
	public static void main(String[] args) {
		int num=0;
		fun(num);
		System.out.println(num);//输出0
	}
}
```

```java
public class Test{
	static void fun(String s) {s="world";}
	public static void main(String[] args) {
		String string="hello";
		fun(string);
		System.out.println(string);//输出 hello
	}
}
```

### 方法重载

方法名相同但是参数列表不同

xxxxxxxxxx1 1conda create -n new_env --clone exist_envsh

#### 使用细节

1. 方法名必须相同
2. 参数列表必须不同(参数类型或个数或顺序，至少有一样不同)
3. 返回类型没有要求

## final

### 修饰变量

此变量需在定义时或在类构造函数中初始化

### 修饰方法

此方法可被子类继承，但不能被子类覆写

### 修饰类

此类不能被继承

## 访问权限

- public:公开的，整体可见

- private:只能被类自身访问

- protected:只能被以下三种之一访问

  1. 该类自己

  2. 同包的其他类

  3. 其他包中该类的子类

- 缺省:可被同包的其他类访问

## `==`与`equals`

如果是基本数据类型，==判断的是值

如果是对象类型，==判断的是对象的地址

通过直接赋值而不是new的方式给String赋值，如果字符串常量池中有该对象，则不会再创建，此时通过 == 判断，返回的是true。如：String str=“wo”；String str1=“wo”;str == str1为true.

```java
public class Test{
	public static void main(String[] args) {
		String s1="hello";
		String s2="hello";
		String s3=new String("hello");
		System.out.println(s1==s2);	//true
		System.out.println(s1==s3);	//false
		System.out.println(s1.equals(s3));//true
		int num1=1;
		int num2=1;
		System.out.println(num1==num2);	//true
	}
}
```

## 数据类型转换

### 基本数据类型转字符串

```java
public class Test{
	public static void main(String[] args) {
		String str1=String.valueOf(123);
		String str2=String.valueOf(true);
		String str3=Integer.toString(456);
		System.out.println(str1);
		System.out.println(str2);
		System.out.println(str3);
	}
}
```

### 字符串转基本数据类型

```java
public class Test{
	public static void main(String[] args) {
		int num1=Integer.valueOf("123");
		int num2=Integer.parseInt("456");
		System.out.println(num1);
		System.out.println(num2);
	}
}
```

## 异常

### try-catch-finally

```java
import java.util.Scanner;

public class Test {
	public static void main(String[] args) {
        int a = 0, b = 0;
        try (Scanner scanner = new Scanner(System.in)) {
            a = scanner.nextInt();
            b = scanner.nextInt();
            System.out.println(a / b);
        } catch (ArithmeticException exception) {
        	System.out.println("除数为0");
        } finally {
        	System.out.print("程序结束");
		}
    }
}
```

### throw-throws

```java
import java.util.Scanner;

public class Test {
	static double div(int num1,int num2) throws ArithmeticException{
		if(num2==0) {
			throw new ArithmeticException();
		}
		else {
			return num1/num2;
		}
	}
	public static void main(String[] args) {
        int a = 0, b = 0;
        try (Scanner scanner = new Scanner(System.in)) {
            a = scanner.nextInt();
            b = scanner.nextInt();
            System.out.println(div(a,b));
        } catch (ArithmeticException exception) {
        	System.out.println("除数为0");
        	exception.printStackTrace();
        } finally {
        	System.out.print("程序结束");
		}
    }
}
```

## javadoc

```java
/**
 * @author 小明
 * @version 1.0
 */
public class Test {

	/**
	 * 求输入两个参数的和
	 * 
	 * @param m 加数1
	 * @param n 加数2
	 * @return 和
	 */
	public static int add(int m, int n) {
		return m + n;
	}
}

```

## 继承

### 示例

```java
class Animal {
	String id;
	void eat() {
		System.out.println("animal : eat");
	}
	public Animal(String id) {
		super();
		this.id = id;
		// some code
	}
	
}
 
class Dog extends Animal {
	void eat() {
		System.out.println("dog : eat");
	}

	public Dog(String id) {
		super(id);
		// some code
	}
}
 
public class Test {
	public static void main(String[] args) {
		Animal a = new Animal("animal");
		a.eat();
		Dog d = new Dog("dog");
	}
}
```

### 注意

子类必须调用父类的构造器，完成父类的初始化。（在子类的构造器中的第一个语句默认有`super();`它默认去调用父类的无参构造器）。这个父类的构造函数必须写在子类构造函数的第一行

### 覆写

子类写一个方法名，参数列表，返回类型与父类相同的方法

返回类型也要一样

```java
class Animal {
	String id;
	void eat() {
		System.out.println("animal : eat");
	}
	public Animal(String id) {
		super();
		this.id = id;
		// some code
	}
	
}
 
class Dog extends Animal {
	String eat() {//报错:The return type is incompatible with Animal.eat()
		System.out.println("dog : eat");
		return "eat";
	}

	public Dog(String id) {
		super(id);
		// some code
	}
}
 
public class Test {
	public static void main(String[] args) {
		
	}
}
```

### 静态类方法和类属性的继承

```java
class Animal {
	static String name = "Animal : name";
	public static void eat() {
		System.out.println("animal : eat");
	}
	public static void sleep() {
		System.out.println("animal : sleep");
	}
}

class Dog extends Animal {
	static String name = "Dog : name";
	public static void eat() {
		System.out.println("dog : eat");
	}
}

public class Test {
	public static void main(String[] args) {
		Animal a = new Dog();
		Dog d = new Dog();

		Dog.eat();	// dog : eat
		a.eat();	// animal : eat
		d.eat();	// dog : eat
		System.out.println(a.name);	// Animal : name
		System.out.println(d.name);	// Dog : name
		System.out.println(Dog.name);// Dog : name
		Dog.sleep();// 这行可以运行 输出结果是 animal : sleep
	}
}
```

#### 结论

得出如下结论:父类中的静态成员变量和方法是可以被子类继承的,但是不能被自己重写,无法形成多态.

#### 原因

**静态方法绑定类，在类加载时便与该类捆绑，不受其他类影响**

**而动态方法绑定实例对象，受子类重写的影响，若被重写即绑定到重写它的那个实例对象上**

## 列表

### 基本操作

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Test {
	public static void main(String[] args) {
        /*初始化*/
		List<Integer> l = new ArrayList<Integer>();
        /*添加*/
		l.add(1);
		l.add(2);
		l.add(3);
		l.add(4);
        /*获取*/
		System.out.println(l.get(1)); // 2
        /*删除*/
		l.remove(1);
        /*遍历*/
		Iterator<Integer> i=l.iterator();
		while(i.hasNext()) {
			System.out.println(i.next());// 1 3 4
		}
	}
}
```

### 三种遍历列表的方法

```java
		List<Product> l_Products=new ArrayList<Product>();	
//choose one of the three following methods
		//for-each
		for(Product l_Product:l_Products)
		{
			System.out.println(l_Product);
		}
		
		//ArrayList<>.get
		for (int i=0;i<l_Products.size();i++)
		{
			System.out.println(l_Products.get(i));
		}

		
		//Iterator
		Iterator<Product> iterator_Product=l_Products.iterator();
		while(iterator_Product.hasNext())
		{
			System.out.println(iterator_Product.next());
		}
```

## 泛型

### 示例

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
class Generic<T> {
	// key这个成员变量的类型为T,T的类型由外部指定
	private T key;

	public Generic(T key) { // 泛型构造方法形参key的类型也为T，T的类型由外部指定
		this.key = key;
	}

	public T getKey() { // 泛型方法getKey的返回值类型为T，T的类型由外部指定
		return key;
	}
}

public class Test {
	public static void main(String[] args) {
		// 泛型的类型参数只能是类类型（包括自定义类），不能是简单类型
		// 传入的实参类型需与泛型的类型参数类型相同，即为Integer.
		Generic<Integer> genericInteger = new Generic<Integer>(2023);
		// 也可以这样定义，但是会警告，所以还是尽量避免
		Generic t1 = new Generic<Integer>(2024);
		Generic t2 = new Generic(2025);

		// 传入的实参类型需与泛型的类型参数类型相同，即为String.
		Generic<String> genericString = new Generic<String>("hello");
		System.out.println("key is " + genericInteger.getKey());
		System.out.println("key is " + genericString.getKey());
		System.out.println("key is " + t1.getKey());
		System.out.println("key is " + t2.getKey());
	}
}
```

### 初始化问题

```java
import java.util.ArrayList;
import java.util.List;

public class Test {
	public static void main(String[] args) {
		//List l1=new ArrayList<int>();// 无法被正确初始化
		List l2=new ArrayList();
		ArrayList l3=new ArrayList<String>();
		//ArrayList<> l4=new ArrayList<String>();// 无法被正确初始化
		ArrayList<String> l5=new ArrayList<String>();
		List<String> l6=new ArrayList<String>();
		List<String> l7=new ArrayList<>();
		//List<> l8=new ArrayList<>();// 无法被正确初始化
		List l9=new ArrayList<>();
	}
}
```

总结，泛型中不能填基本数据类型，`=`号前面不能有空的`<>`，`=`号后面可以进行一些省略(会被警告)，但是最好还是不要偷懒，要写规范，否则会出一些问题

### 定义方法

#### 泛型类

语法：修饰符 Class 类名称 <泛型类型>

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
class Generic<T> {
	// key这个成员变量的类型为T,T的类型由外部指定
	private T key;

	public Generic(T key) { // 泛型构造方法形参key的类型也为T，T的类型由外部指定
		this.key = key;
	}

	public T getKey() { // 泛型方法getKey的返回值类型为T，T的类型由外部指定
		return key;
	}
}

public class Test {
	public static void main(String[] args) {
		// 泛型的类型参数只能是类类型（包括自定义类），不能是简单类型
		// 传入的实参类型需与泛型的类型参数类型相同，即为Integer.
		Generic<Integer> genericInteger = new Generic<Integer>(2023);
		// 传入的实参类型需与泛型的类型参数类型相同，即为String.
		Generic<String> genericString = new Generic<String>("hello");
		System.out.println("key is " + genericInteger.getKey());
		System.out.println("key is " + genericString.getKey());

	}
}
```

#### 泛型方法

泛型方法的格式：修饰符 <泛型变量> 方法返回值 方法名称(形参列表){方法体}

```java
public class Test {
	public static <T> void show(T t) {System.out.println(t);}
	public static void main(String[] args) {
		show(123);
	}
}
```

#### 泛型接口

泛型接口的格式：修饰符 interface 接口名称<泛型变量>{}

##### 实现类也是泛型类

若实现类也是泛型类，实现类和接口的泛型类型要一致

```java
/**
 * 泛型接口
 * @param <T>
 */
public interface Generator<T> {
    T getKey();
}
/**
 * 泛型接口的实现类，是一个泛型类，
 * 那么要保证实现接口的泛型类泛型标识包含泛型接口的泛型标识
 * @param <T>
 * @param <E>
 */
public class Pair<T,E> implements Generator<T> {
 
    private T key;
    private E value;
 
    public Pair(T key, E value) {
        this.key = key;
        this.value = value;
    }
 
    @Override
    public T getKey() {
        return key;
    }
 
    public E getValue() {
        return value;
    }
}
 
```

##### 实现类不是泛型类

若实现类不是泛型类，接口要明确数据类型

```java
/**
 * 实现泛型接口的类，不是泛型类，需要明确实现泛型接口的数据类型。
 */
public class Apple implements Generator<String> {
    @Override
    public String getKey() {
        return "hello generic";
    }
}
```

## 数组

初始化及遍历

注意：给数组分配空间时，必须指定数组能够存储的元素个数来确定数组大小。创建数组之后不能修改数组的大小。可以使用length 属性获取数组的大小。

```java
public class Test {
	public static void main(String[] args) {
		/*初始化*/
		int a[]=new int[5];
		int[] b=new int[]{3,5,1,7};
		int[] c={3,5,1,7};
		//注意：给数组分配空间时，必须指定数组能够存储的元素个数来确定数组大小。
        //创建数组之后不能修改数组的大小。可以使用length 属性获取数组的大小。
		//int d[];d= {1,2,3};//错误
		//int e[5]=new int[5];//错误
        
		/*遍历，使用length属性*/
		for(int i=0;i<b.length;i++) {
			System.out.println(b[i]);
		}
	}
}
```

## 抽象


- 抽象类不能被`new`实例化
- 任何子类必须重写父类的抽象方法，或者声明自身为抽象类。
- 构造方法，类方法（用 static 修饰的方法）不能声明为抽象方法
- 抽象类中不一定包含抽象方法，但是有抽象方法的类必定是抽象类
- 抽象类实现接口，可以不实现接口的方法

```java
abstract class Shape {
    public int width; // 几何图形的长
    public int height; // 几何图形的宽

    public Shape(int width, int height) {
        this.width = width;
        this.height = height;
    }

    public abstract double area(); // 定义抽象方法，计算面积
}

class Square extends Shape {
    public Square(int width, int height) {
        super(width, height);
    }
    // 重写父类中的抽象方法，实现计算正方形面积的功能
    @Override
    public double area() {
        return width * height;
    }
}

public class Test {
	public static void main(String[] args) {
		 Square square = new Square(5, 4); // 创建正方形类对象
	     System.out.println("正方形的面积为：" + square.area());
	}
}
```

## 接口

### 示例

```java
interface Animal {
	public void eat();
	public void travel();
}

class MammalInt implements Animal {
	public void eat() {
		System.out.println("Mammal eats");
	}
	public void travel() {
		System.out.println("Mammal travels");
	}
	public int noOfLegs() {
		return 0;
	}
}

public class Test {
	public static void main(String args[]) {
		// 可以用接口来指向实现了的类的示例
		Animal m = new MammalInt();
		//MammalInt m = new MammalInt();//当然也可以这样定义
		m.eat();
		m.travel();
	}
}
```

### 与class的区别

- **接口不能用于实例化对象。**

- **接口没有构造方法。**

- **接口中所有的方法必须是抽象方法**，Java 8 之后 接口中可以使用 default 关键字修饰的非抽象方法。

  ​	(接口中每一个方法也是隐式抽象的,接口中的方法会被隐式的指定为 **public abstract**)

- **接口不能包含成员变量**，除了 static 和 final 变量。

- 接口不是被类继承了，而是要被类实现。

- 接口支持多继承。

### 接口的继承

```java
public interface Sports
{
   public void setHomeTeam(String name);
   public void setVisitingTeam(String name);
}
 
// 文件名: Football.java
public interface Football extends Sports
{
   public void homeTeamScored(int points);
   public void visitingTeamScored(int points);
   public void endOfQuarter(int quarter);
}

/*接口可以多继承，而类不能
	public interface Hockey extends Sports, Event
*/
```

## 字符串分割

使用String类中的`split`方法分割

 String[] split(String regex) 
          根据给定正则表达式的匹配拆分此字符串。 

```java
public class Test {
	public static void main(String args[]) {
		String sa[]="hello_world_!".split("_");
		for(int i=0;i<sa.length;i++) {
			System.out.println(sa[i]);
		}
	}
}
```

StringTokenizer类

```java
     StringTokenizer st = new StringTokenizer("this is a test");
     while (st.hasMoreTokens()) {
         System.out.println(st.nextToken());
     }

```

## Set

一个接口，需要实现`add`和`remove`和`size`等方法(不要求实现`get`)，不包含重复元素。正如其名称所暗示的，此接口模仿了数学上的 *set* 抽象。

实现了它的主要有`HashSet`和`TreeSet`等

```java
import java.util.HashSet;

public class Test {
	public static void main(String args[]) {
		HashSet<String> hs = new HashSet<>(); // 创建HashSet对象
		boolean b1 = hs.add("a");
		boolean b2 = hs.add("a"); // 当向set集合中存储重复元素的时候返回为false

		hs.add("b");
		hs.add("c");
		hs.add("d");
		System.out.println(hs); // [d, b, c, a] 存取无序 并且去掉了重复元素
		System.out.println(b1); // true
		System.out.println(b2); // false
		hs.remove("b");
		for (String string : hs) { // 只要能用迭代器迭代的,就可以使用增强for循环遍历
			System.out.println(string);// a c d
		}
	}
}
```

## Map

一个接口，需要实现`get`和`put`和`size`等方法

实现了它的主要有`HashMap`和`TreeMap`等

```java
import java.util.HashMap;
import java.util.Map;

public class Test {
	public static void main(String args[]) {
		//创建Map对象
        Map<String, String> map = new HashMap<String,String>();       //数据采用的哈希表结构
        //给map中添加元素
        map.put("星期一", "Monday");
        map.put("星期日", "Sunday");
        System.out.println(map); // {星期日=Sunday, 星期一=Monday}
 
        //当给Map中添加元素，会返回key对应的原来的value值，若key没有对应的值，返回null
        System.out.println(map.put("星期一", "Mon")); // Monday
        System.out.println(map); // {星期日=Sunday, 星期一=Mon}
 
        //根据指定的key获取对应的value
        String en = map.get("星期日");
        System.out.println(en); // Sunday
        
        //根据key删除元素,会返回key对应的value值
        String value = map.remove("星期日");
        System.out.println(value); // Sunday
        System.out.println(map); // {星期一=Mon}
        
        /* 修改对应的值
        使用replace(key,newValue)，查找出HashMap中，指定的key的curValue，
        如果replace的入参oldValue和curValue相等，则执行put(key,newValue)，把入参newValue替换掉原来对应的值。
        */
	}
}
```

## 变量默认值

```java
public class Test {
	static String str;
	static int i;
	static boolean b;
	static char c;
    public static void main(String[] args) {
    	System.out.println(str);// null
    	System.out.println(i);	// 0
    	System.out.println(b);	// false
    	System.out.println(c);	// '\0'
    	
    }
}
```

## java内存的结构

Java程序在运行时，需要在内存中的分配空间。为了提高运算效率，就对数据进行了不同空间的划分，因为每一片区域都有特定的处理数据方式和内存管理方式。

**具体划分为如下5个内存空间：**

- 栈：存放局部变量
- 堆：存放所有new出来的东西
- 方法区：被虚拟机加载的类信息、常量、静态常量等。
- 程序计数器(和系统相关)
- 本地方法栈

## 查漏补缺

1. char所占空间为2字节，因为unicode
2. 全局变量可以不赋值，直接使用，因为有默认值，局部变量必须赋值后才能使用，因为没有默认值。
3. 
