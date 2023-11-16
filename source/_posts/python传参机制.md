---
title: python传参机制
date: 2023-08-08 09:07:58
tags: python
categories: 
  - note
excerpt: python和C/C++/Java等语言的传参机制有所不同
---

*这篇笔记只是个人拙见，可能有不对的地方，还请海涵*

# Python传参机制

> Python 中一切皆为对象，数字是对象，列表是对象，函数也是对象，任何东西都是对象。而变量只是对象的一个引用，对象的操作都是通过引用来完成的

个人认为，python函数内重新赋值时，不会改变函数外的值，但在函数内修改成员变量的值时，会改变函数外的值

## python内存分配机制

这个机制让我想起了Java的字符串分配内存的情形

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

放一段代码以做对照

### 对于不可变对象
关于不可变对象（数字、字符或元组），来看一个例子
```python
a=1
b=1
c=1
print(id(a),id(b),id(c))  # 140706301014824 140706301014824 140706301014824
b=2
c+=1
print(id(a),id(b),id(c))  # 140706301014824 140706301014856 140706301014856

```

可以看出，值相同，就是指向一样的内存地址

### 对于可变对象

关于可变对象（字典、列表），来看一个例子

```python
a=[1,2]
b=[1,2]
c=[1,2]
print(id(a),id(b),id(c))  # 140706301014824 140706301014824 140706301014824
b.append(3) # [1, 2, 3]
c.append(3) # [1, 2, 3]
print(id(a),id(b),id(c))  # 140706301014824 140706301014856 140706301014856

```

修改其中的值后，会指向新的对象

## 对于赋值

赋值会为新的值开辟新的空间

```python
def fun(x):
    print('赋值前x的地址是'+str(id(x))) # 140706301014824 x和a指向的位置一样
    x=2
    print('赋值后x的地址是'+str(id(x))) # 140706301014856 x的指向改变，不再指向1


a=1
print('赋值前a的地址是'+str(id(a))) # 140706301014824
fun(a)
print(a)  # 1
```
即使是实参是列表也是一样
```python
def fun(x):
    print('赋值前x的地址是'+str(id(x))) # 140706301014824
    x=2
    print('赋值后x的地址是'+str(id(x))) # 140706301014856


a=[1]
print('赋值前a的地址是'+str(id(a))) # 140706301014824
fun(a)
print(a)  # 1
```

## 对于修改值

修改值会影响函数外面的值

```python
def func(x):
    x['a'] = 1
    x['b'] = 2      
    print(x)  # {'a': 1, 'b': 2}
    x = {'a': 3, 'b': 4}
    print(x)  # {'a': 3, 'b': 4}


t = {}
func(t)
print(t)  # {'a': 1, 'b': 2}

```

看一个类的例子

```python
class Test:
    def __init__(self) -> None:
        x=1
    
    def __call__(self):
        print(self.x)

def fun(t:Test):
    t.x=2
    t()

a=Test()
fun(a)  # 2

```

也是一样的

# 修改全局变量

顺便说一下怎么修改全局变量

直接修改是修改不了的

```python
def fun():
    a=2

a=1
fun()
print(a) # 1
```

事实上，修改不了的原因应该在于函数内部是无法直接访问全局变量的

```python
a=1  # 可以看到，即使把a提到最前面也会报错
def fun():
    print(a)  # UnboundLocalError: cannot access local variable 'a' where it is not associated with a value
    a=2

fun()
```

想要修改的话，可以使用`global`这个关键字

```python
def fun():
    # 下面这种写法是错误的
    global a=2  # SyntaxError: invalid syntax

a=1
fun()
print(a)
```

像这样就修改成功了

```python
def fun():
    global a
    a=2

a=1
fun()
print(a) # 2
```

