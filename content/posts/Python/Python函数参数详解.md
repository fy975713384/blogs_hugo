---
title: 'Python 函数参数详解'
date: 2019-10-06T16:14:18+08:00
lastmod: 2020-11-18T14:17:18+08:00
tags: ['Python']
categories: ['Notes']
authors:
  - '潘峰'
---

# Python 函数参数详解

## 前言

> 最近在跟同事讨论一个方法的调用时涉及到了 Python 函数的位置参数和关键字参数的相关知识，发现之前学习 Python 时对函数参数研究的并不透彻，且很多地方已经有些生疏了，故而查阅了一下 Python 官方文档和廖雪峰的官网中的相关知识，并总结记录如下。

## Argument 和 Parameter

Python 函数参数根据使用情况的不同需要分为 Parameter 和 Argument 两部分进行讨论。

### Argument

> Argument 指的是函数调用时的实际参数，即实参 (actual parameter)，也可以称为引数。
> Python 中有两种 Argument，分别是「位置参数」和「关键字参数」

#### 位置参数 [positional argument]

位置参数使用时可以放在参数列表的开头，和/或是以一个带有 `*` 前缀的可迭代的元素表示，以内置函数 complex() 的调用为例：

```python
complex(3, 5)
complex(*(3, 5))
```

_`*` 表示将可迭代对象扩展为函数的参数列表_

#### 关键字参数 [keyword argument]

关键字参数使用时需要用标识符指明（`name=` 的形式），或是以一个带有 `**` 前缀的字典表示，以内置函数 complex() 的调用为例：

```python
complex(real=3, imag=5)
complex(**{'real': 3, 'imag': 5})
```

_`**` 表示将字典扩展为函数的关键字参数_

### Parameter

> Parameter 指的是函数定义时的形式参数，即形参 (formal parameter)。Python 中有五种 Parameter，分别是「位置或关键字参数」、「仅位置参数」、「仅关键字参数」、「可变位置参数」、「可变关键字参数」

#### 位置或关键字参数 [positional-or-keyword]

位置或关键字参数在函数调用时可以以位置参数 (positional argument) 或关键字参数 (keyword argument) 的形式提供。它是默认的参数类型。

```python
def func(foo1, foo2=None): ...
```

其中 foo1 也可称为非默认参数 [non-default parameter]；foo2 可称为默认参数 [default parameter]，默认参数带有默认值，可以简化函数调用。

注意，在函数定义时非默认参数必须在默认参数之前。

#### 仅位置参数 [positional-only]

仅位置参数在函数调用时只能由位置参数 (positional argument) 提供。Python 没有提供定义该参数的语法，它只在一些内置函数中存在，例如 `abs()`。

#### 仅关键字参数 [keyword-only]

仅关键字参数在函数调用时只能由关键字参数 (keyword argument) 提供，它可以对函数传入的关键字参数进行限制。
仅关键字参数定义时需要在其之前紧邻一个可变位置参数或增加一个特殊分割符 `*`。

1. 含有可变位置参数时以可变位置参数为分割，可变位置参数后都是仅关键字参数

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

可以这样调用 ta

```python
test_arg('beijing','wfp',age='25',job='hacker')
test_arg('beijing','wfp',addr='shanghai',age='25',job='hacker')
```

但是不能这样调用 ta

```python
test_arg('beijing','wfp','25',job='hacker')
```

> 会提示缺少一个参数，定义了命名关键字参数的话，必须要把全部的关键字参数传入进去

2. 没有可变位置参数时，增加一个 `*` 作为特殊分隔符，`*` 后面的都是仅关键字参数

```python
def test_arg(city,name,*,age,job):
    print(name, age, args, city, job)
```

_`*` 作为特殊分割符使用_

仅关键字参数可以设置默认值，从而简化调用：

```python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```

由于参数 city 具有默认值，调用时，可不传入 city 参数：

```shell
>>> person('Jack', 24, job='Engineer')
Jack 24 Beijing Engineer
```

> 使用命名关键字参数时，要特别注意，如果没有可变参数，就必须加一个 。如果缺少 `*` Python 解释器将无法识别 positional-or-keyword 和 keyword-only

#### 可变位置参数 [var-positional]

可变参数很简单，在 C/C++ 和 Java 等语言中都有，就是用 `*args` 号来表示，例如

```python
def test_arg(*arg): ...
```

_`*` 表示将函数调用时的多个参数打包成一个元组_

你可以传入任意多个元素（包括 0）到参数中，在函数调用时会自动被认为是一个元组

#### 可变关键字参数 [var-keyword]

可变关键字参数在 python 中习惯用 `**kwargs` 表示，可以传入 0 到任意多个“关键字-值”，参数在函数内部被当做一个字典

```python
def test_arg(**kwargs): ...
def test_arg(city, **kwargs): ...
```

_`**` 表示将函数调用时的多个关键字参数打包成一个字典_

可以这样调用它

```python
test_arg(name='John', job='hacker')
test_arg('beijing', name='john')
```

关键字参数可以用来后期扩充函数的功能，例如：先设定必要的参数，之后选择性的增加可选参数。

#### Parameter 组合使用时的顺序

位置或关键字参数-非默认参数 >> 位置或关键字参数-默认参数 >> 可变位置参数 >> 仅关键字参数 >> 可变关键字参数

## 函数参数传递

> 在编程语言中常见的函数参数传递方式有以下两种：  
> 值传递：调用函数时将实参的值拷贝一份传递给形参，在函数中如果对形参进行修改不会影响到实参。
> 引用传递：调用函数时将实参的值的地址传递给形参，在函数中如果对形参进行修改将同时影响到实参。  
> _注：Python 中形参会作为局部变量存放在函数的 Local 命令空间中_

**先说结论：Python 函数传参使用的是引用传递。由于在 Python 中一切皆对象，实际传递的都是对象的引用。**

### 不可变对象传参

示例：

```python
def test(par):
    print("par:", id(par))


arg = 1
print("arg:", id(arg))
test(arg)


# 运行结果如下
arg: 4331100528
par: 4331100528
```

Python 中的内置函数 `id()` 可以查看对象的内存地址，从该示例中可以看出，调用 `test()` 方法时是将实参变量 `arg` 的引用传递给了形参变量 `par`，因此 `arg` 和 `par` 地址相同。  
由于 `arg` 的值 1 是不可变对象，所以在函数内部也无法对其进行修改。

### 可变对象传参

#### 修改形参引用的可变对象

示例：

```python
def test(par):
    print("par:", id(par))
    par.append(1)


arg = [0]
print("[Before]", f"arg: {id(arg)}", arg, sep='\n')
test(arg)
print("[After]", f"arg: {id(arg)}", arg, sep='\n')


# 运行结果如下：
[Before]
arg: 4462379136
[0]
par: 4462379136
[After]
arg: 4462379136
[0, 1]
```

从该示例中可以看出，对于可变对象，调用 `test()` 方法时仍是将实参变量 `arg` 的引用传递给了形参变量 `par`，因此 `arg` 和 `par` 地址相同，并且在函数内部修改 `par` 引用的对象时会同时修改 `arg` 引用的对象。

#### 修改形参的引用

示例：

```python
def test(par):
    print("par:", id(par))
    par = [1]


arg = [0]
print("[Before]", f"arg: {id(arg)}", arg, sep='\n')
test(arg)
print("[After]", f"arg: {id(arg)}", arg, sep='\n')
```

从该示例中可以看出，当我们在函数内部修改形参变量 `par` 的引用时，并不会影响到实参变量 `arg` 引用的对象，这是因为我们只是将 `par` 引用的对象由 `[0]` 改为 `[1]`，并没有修改原对象 `[0]`，所以 `arg` 打印出来的值仍为 `[0]`。  
_注：在 Python 中，赋值操作 `par = [1]` 本质上是建立了变量 `par` 与对象 `[1]` 的引用关系。_

> **参考来源**：  
> https://docs.python.org/3.6/glossary.html#term-parameter  
> https://docs.python.org/3.6/glossary.html#term-argument  
> https://docs.python.org/3.6/faq/programming.html#faq-argument-vs-parameter  
> https://www.liaoxuefeng.com/wiki/1016959663602400/1017261630425888
