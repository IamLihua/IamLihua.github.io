---
title: python装饰器
date: 2023-11-09 16:17:15
tags: python
categories: 
  - note
excerpt: 简单学习一下python的装饰器，写篇简单的笔记。
---

# python装饰器

## 函数内的函数定义

```python
def f1(a):
    def f2(b):
        return a + b
    return f2

# 计算3+4=7
t = f1(3) # 这里返回的是一个函数
print(t(4)) # 7
```

## 一个简单的装饰器

```python
import time

def timer(fun_to_run):
    """
    输入参数是一个函数
    返回值也是一个函数
    """
    def wrapper():
        t_begin = time.time()
        fun_to_run()
        t_end = time.time()
        print(f'函数{fun_to_run.__name__}的运行时间为: {t_end - t_begin} s')
    return wrapper

@timer
def my_func():
    r=0
    for i in range(1000000):
        r+=i
    print(r)

my_func() # 函数my_func的运行时间为: 0.04650068283081055 s
```

相当于

```python
import time

def timer(fun_to_run):
    """
    输入参数是一个函数
    返回值也是一个函数
    """
    def wrapper():
        t_begin = time.time()
        fun_to_run()
        t_end = time.time()
        print(f'函数{fun_to_run.__name__}的运行时间为: {t_end - t_begin} s')
    return wrapper

def my_func():
    r=0
    for i in range(1000000):
        r+=i
    print(r)

t=timer(my_func)
t() # 函数my_func的运行时间为: 0.04343557357788086 s
```

## 在装饰器中使用函数参数

按照一定的顺序进行定义，如下面的代码所示

```python
import time

def timer(fun_to_run):
    # 第一个参数是要装饰的函数
    def wrapper(*args,**kargs):
        # 第二个参数是要装饰的函数的参数
        t_begin = time.time()
        fun_to_run(*args,**kargs)
        t_end = time.time()
        print(f'函数{fun_to_run.__name__}的运行时间为: {t_end - t_begin} s')
    return wrapper

@timer
def my_func(num):
    r=0
    for i in range(num):
        r+=i
    print(r)

my_func(5555) # 函数my_func的运行时间为: 0.04650068283081055 s
```

## 带参数的函数装饰器

按照一定的顺序进行定义，如下面的代码所示

```python
import time

def timer(unit):
    # 第一个参数是装饰器参数
    def middle(fun_to_run):
        # 第二个参数是要装饰的函数
        def wrapper(*args, **kwargs):
            # 第三个参数是要装饰的函数的参数
            t_begin = time.time()
            fun_to_run(*args, **kwargs)
            t_end = time.time()
            if unit == "ms":
                print(f'函数{fun_to_run.__name__}的运行时间为: {(t_end - t_begin)*1000} ms')
            elif unit == "s":
                print(f'函数{fun_to_run.__name__}的运行时间为: {t_end - t_begin} s')
            else:
                return
        return wrapper
    return middle

@timer("ms")
def my_func(num):
    r=0
    for i in range(num):
        r+=i
    print(r)

my_func(500000) # 函数my_func的运行时间为: 0.0 ms
```

## 使用被装饰的函数的返回值

这样装饰器就不是返回一个函数了，而是返回对应的返回值

```python
import time

def timer(fun_to_run):
    # 第一个参数是要装饰的函数
    def wrapper(*args,**kargs):
        # 第二个参数是要装饰的函数的参数
        t_begin = time.time()
        result =fun_to_run(*args,**kargs)
        t_end = time.time()
        print(f'函数{fun_to_run.__name__}的运行时间为: {t_end - t_begin} s')
        # 要在wrapper函数设定返回值
        return result
    return wrapper

@timer
def my_func(num):
    r=0
    for i in range(num):
        r+=i
    return r

res = my_func(5555) # 函数my_func的运行时间为: 0.0 s
print(res) # 15426235
```

如果按老样子，不改的话，就会输出以下内容，这是因为`wrapper`函数没有返回值

```python
import time

def timer(fun_to_run):
    # 第一个参数是要装饰的函数
    def wrapper(*args,**kargs):
        # 第二个参数是要装饰的函数的参数
        t_begin = time.time()
        result = fun_to_run(*args,**kargs)
        t_end = time.time()
        print(f'函数{fun_to_run.__name__}的运行时间为: {t_end - t_begin} s')
    return wrapper

@timer
def my_func(num):
    r=0
    for i in range(num):
        r+=i
    return r

res = my_func(5555) # 函数my_func的运行时间为: 0.0 s
print(res) # None
```

## 其他

此外也有类装饰器等，可以以后有需要了再学。
