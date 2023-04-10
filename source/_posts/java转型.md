---
title: java转型
date: 2023-03-22 22:29:20
tags: java
categories: 
  - note
---

# 向上转型与向下转型

类型转换只是转换看待对象的引用的类型，对象本身没有也不可能参与转换

## 向上转型

父类引用可以自动指向子类对象，但只能访问和调用到来自于父类的属性和行为

调用属性看父类，调用方法先看子类，子类没有，再看父类，如下代码

```java
class Father{
	public String name="father";
	public void fun() {
		System.out.println("Father fun");
	}
}

class Son extends Father{
	public String name="son";
	public void fun() {
		System.out.println("Son fun");
	}
}

public class Test{
	public static void main(String[] args) {
		Father f=new Son(); 
		System.out.println(f.name); // 输出 "father"
		f.fun();// 输出 "Son fun"
	}
}
```

## 向下转型

把父类引用赋给子类引用，语法上必须使用强制类型转换，要想运行也成功还必须保证父类引用指向的对象一定是该子类对象（最好使用instance判断后，再强转）

## 示例

参考网址：[Java对象类型转换：向上转型和向下转型 ](http://c.biancheng.net/view/6503.html)

```java
public class Animal {
    public String name = "Animal：动物";
    public static String staticName = "Animal：可爱的动物";

    public void eat() {
        System.out.println("Animal：吃饭");
    }

    public static void staticEat() {
        System.out.println("Animal：动物在吃饭");
    }
}

public class Cat extends Animal {
    public String name = "Cat：猫";
    public String str = "Cat：可爱的小猫";
    public static String staticName = "Dog：我是喵星人";

    public void eat() {
        System.out.println("Cat：吃饭");
    }

    public static void staticEat() {
        System.out.println("Cat：猫在吃饭");
    }

    public void eatMethod() {
        System.out.println("Cat：猫喜欢吃鱼");
    }

    public static void main(String[] args) {
        //Cat cat = new Animal();			// 出错
        Animal animal = new Cat();
        Cat cat = (Cat) animal; 			// 向下转型
        System.out.println(animal.name); 	// 输出Animal类的name变量
        System.out.println(animal.staticName); // 输出Animal类的staticName变量
        animal.eat(); 		// 输出Cat类的eat()方法
        animal.staticEat(); // 输出Animal类的staticEat()方法
        System.out.println(cat.str); // 调用Cat类的str变量
        cat.eatMethod(); 	// 调用Cat类的eatMethod()方法
    }
}
```

## 强制对象类型转换

Java 编译器允许在具有直接或间接继承关系的类之间进行类型转换。对于向下转型，必须进行强制类型转换；对于向上转型，不必使用强制类型转换。

例如，对于一个引用类型的变量，Java 编译器按照它声明的类型来处理。如果使用 animal 调用 str 和 eatMethod() 方法将会出错，如下：

```java
animal.str = "";    // 编译出错，提示Animal类中没有str属性
animal.eatMethod();    // 编译出错，提示Animal类中没有eatMethod()方法
```


如果要访问 Cat 类的成员，必须通过强制类型转换，如下：

```java
((Cat)animal).str = "";    // 编译成功
((Cat)animal).eatMethod();    // 编译成功
```


把 Animal 对象类型强制转换为 Cat 对象类型，这时上面两句编译成功。对于如下语句，由于使用了强制类型转换，所以也会编译成功，例如：

```java
Cat cat = (Cat)animal;    // 编译成功，将Animal对象类型强制转换为Cat对象类型
```


类型强制转换时想运行成功就必须保证父类引用指向的对象一定是该子类对象，最好使用 instanceof 运算符判断后，再强转，例如：

```java
Animal animal = new Cat();
if (animal instanceof Cat) 
{    
    Cat cat = (Cat) animal; // 向下转型    ...
}
```


子类的对象可以转换成父类类型，而父类的对象实际上无法转换为子类类型。因为通俗地讲，父类拥有的成员子类肯定也有，而子类拥有的成员，父类不一定有。因此，对于向上转型，不必使用强制类型转换。例如：

```java
Cat cat = new Cat();
Animal animal = cat;    // 向上转型，不必使用强制类型转换
```


如果两种类型之间没有继承关系，那么将不允许进行类型转换。例如：

```java
Dog dog = new Dog();
Cat cat = (Cat)dog;    // 编译出错，不允许把Dog对象类型转换为Cat对象类型
```

# 隐式转型与显式转型

## 隐式转型

**隐式转换也叫自动类型转换，指的是不需要调用函数，JVM自动将类型转换的一种方式。因为这种类型转换经常使用，Java语言在设计时，为了减轻开发人员的负担，都交给JVM来自动处理。**

1)转换规则从存储范围小的类型到存储范围大的类型(只有前面的数据才能随便转换为后边的)
byte—> short,char—> int —> long—> float —> double
2)例子：
byte b = 2; short s = b; 首先JVM会将b的值转换为short类型，再将值赋值给s

## 显式转型

**显示转换也叫强制类型转换，指的是需要手动去处理才能完成的类型转换。该转换会存在精度损失。**
1)转换规则从存储范围大的类型到存储范围小的类型
double→float→long→int→short(char)→byte
2)例子：
double d = 1.1; int i = (int)d;
首先将d的值转换成int类型，然后赋值给变量i。需要注意的是小数强制转换为整数，采用的是“去1法”，也就是舍弃小数点后面所有数字，则以上转换出的结果是1。整数强制转换为整数时取数字的低位，例如int类型的变量转换为byte类型时，则只去int类型的低8位(也就是最后一个字节)的值。
