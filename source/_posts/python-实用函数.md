---
title: python实用函数
date: 2023-12-03 18:19:26
tags: python
categories: 
  - note
excerpt: zip enumerate lambda map
---

# python实用函数

## zip

> 合并两个列表

返回值为一个`zip`对象

`zip`中的每个元素都是一个元组

```python
# zip是将两个列表合在一起
a = [1, 2, 3]
b = ['4', '5', '6']
# print(zip(a,b)) # <zip object at 0x00000234EA475140>
for (i, j) in zip(a, b):
    print(i, j)  # 1 4 - 2 5 - 3 6

```

## enumerate

> enumerate会给列表加上序号 从0开始

返回值为一个`enumerate`对象

`enumerate`中的每个元素都是一个元组

```python
# enumerate会给列表加上序号 从0开始
c = ['a', 'b', 'c']
# print(enumerate(c)) # <enumerate object at 0x0000027817E216C0>
for (i, j) in enumerate(c):
    print(i, j)  # 0 a - 1 b - 2 c
```

## map

### 匿名函数

格式: lambda \<args> : \<expression>

参考: [匿名函数 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1016959663602400/1017451447842528)

> 关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数。
>
> 匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。
>
> 用匿名函数有个好处，因为函数没有名字，不必担心函数名冲突。此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数

```python
d=lambda x,y,z:x*y*z
print(d(2,3,4))
print((lambda x:x**2)(3))
```

### map函数

> map()是 Python 内置的高阶函数，它接收一个函数 f 和一个 list，并通过把函数 f 依次作用在 list 的每个元素上，得到一个新的 list 并返回。

```python
map(function, iterable, ...)
```

事实上`map`函数返回的是一个`map`对象

`map`的第一个参数可以写一个已知的函数，也可以写一个**lambda**表达式

```python
def d(x):
    return x + 1


for i in map(d, [1, 2, 3]):
    print(i)

for j in map(lambda x: x ** 2, [2, 3, 4]):
    print(j)
```
