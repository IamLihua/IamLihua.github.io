---
title: Python类型注解
date: 2023-03-22 22:18:45
tags: python
categories: 
  - note
excerpt: python虽然是一种弱类型的语言，但是可以用一些语法让我们写代码的时候注意输入参数类型
---

# Python类型注解

## 学习网址

第1个是详细版，第2个是简单版

- [Python类型注解，你需要知道的都在这里了 - 知乎](https://zhuanlan.zhihu.com/p/419955374)
- [Python类型注解-CSDN](https://blog.csdn.net/mahoon411/article/details/125657457)

### **注意**

类型注解**仅仅**是提供给编辑器进行类型检查的机会，也就是起提示的作用，对 Python 程序的运行不会产生任何影响。也就是说，Python 跟以前一样自由，即使你进行了错误的类型赋值，只要不直接引发错误，程序依旧可以运行。

要有报错的话，需要装一些插件，如vscode的**Pylance**

### 变量

```python
age: int = 20
```

其实就是在变量的后面加`:type`，其他照旧

在指定之后，此变量不应该被赋值为其他类型（不过解释器不会报错），在 **VS Code**中，安装好类型注解插件 **Pylance** 后，如果写出下面的代码：

```python
age: int = 20
age = '20'
```

那么编辑器会用**醒目**的方式告诉你：孙子，你这里的类型写错了！

### 函数

#### 普通函数

```python
def say_hi(name: str) -> str:
    return f'Hello {name}!'
```

在形参上还是如上文一样，`->type`写括号后面代表返回值，如果没有返回值，可以写`->None`

#### 带默认值的函数

```python
def add(first: int = 10, second: float = 5.5) -> float:
    return first + second
```

道理是一样的

### 列表、字典、元组

列表、字典、元组等包含元素的复合类型，用简单的 list，dict，tuple 不能够明确说明内部元素的具体类型。

因此要用到 `typing` 模块提供的**复合注解**功能：

```python
from typing import List, Dict, Tuple

# 参数1: 元素为 int 的列表
# 参数2: 键为字符串，值为 int 的字典
# 返回值: 包含两个元素的元组
def mix(scores: List[int], ages: Dict[str, int]) -> Tuple[int, int]:
    return (0, 0)
```

如果你用的是 Python 3.9+ 版本，甚至连 `typing` 模块都不需要了，内置的容器类型就支持了复合注解：

```python
def mix(scores: list[int], ages: dict[str, int]) -> tuple[int, int]:
    return (0, 0)
```

在某些情况下，不需要严格区分参数到底是列表还是元组（这种情况还蛮多的）。这时候就可以将它们的特征抽象为更泛化的类型（泛型），比如 Sequence（序列）。

下面是例子：

```python
# Python 3.8 之前的版本
from typing import Sequence as Seq1

def foo(seq: Seq1[str]):
    for item in seq:
        print(item)


# Python 3.9+ 也可以这么写
from collections.abc import Sequence as Seq2

def bar(seq: Seq2[str]):
    for item in seq:
        print(item)
```

例子中函数的参数不对容器的类型做具体要求，只要它是个序列（比如列表和元组）就可以。

### Any

如果你实在不知道某个类型注解应该怎么写时，这里还有个最后的逃生通道：

```python
from typing import Any

def foo() -> Any:
    pass
```

任何类型都与 `Any` 兼容。当然如果你把所有的类型都注解为 `Any` 将毫无意义，因此 `Any` 应当尽量少使用。
