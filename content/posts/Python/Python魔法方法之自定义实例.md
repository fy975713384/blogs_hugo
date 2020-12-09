---
title: 'Python 魔法方法之自定义实例'
date: 2020-06-17T12:38:01+08:00
lastmod: 2020-08-11T19:04:14+08:00
tags: ['Python', 'Python魔法方法']
categories: ['Notes']
authors:
  - '潘峰'
---

# Python 魔法方法之自定义实例

> _Python 魔法方法系列 让你的代码更加 pythonic_

## 构造函数 & 析构函数

学习 Python 的自定义实例魔法方法前需要先了解两个概念，**构造函数** 和 **析构函数**。

> 【构造函数】是一种特殊的方法，主要用来在创建对象时初始化对象，即为对象成员变量赋初始值。  
> 【析构函数】与构造函数相反，当对象结束其生命周期，如对象所在的函数已调用完毕时，系统自动执行析构函数。析构函数往往用来做“清理善后” 的工作。  
> ---来源于百度百科

在 Python 中，`__new__()` 和 `__init__()` 方法组合起来扮演了构造函数的角色，而 `__del__()` 方法起到了析构函数的作用。

## \_\_new\_\_

`object.__new__(cls[, ...])` 方法用于实例的创建，使用时需注意：

- 该魔法方法接收被请求创建该实例的类作为其第一个参数，其余参数将被传递给对象构造函数表达式
- 该魔法方法需要返回一个新的对象（通常是 cls）实例，否则 `__init__()` 方法将不会被调用
- 该魔法方法主要目的是给不可变类型的子类提供自定义实例的创建，以及用于自定义元类（用于创建自定义类）

```python
# 通过 __new__() 函数实现单例模式
class Singleton:
    def __new__(cls):
        if not hasattr(cls, 'instance'):
            cls.instance = super(Singleton, cls).__new__(cls)
        return cls.instance
```

## \_\_init\_\_

`object.__init__(self[, ...])` 方法用于实例的初始化，使用时需注意：

- 该魔法方法会在实例被创建后调用
- 参数来源于传递给类构造函数表达式的函数参数
- 该魔法方法不能返回任何非 None 的值
- 如果父类含有该魔法方法，则子类的该魔法方法中必须显式地调用父类的该魔法方法（以确保实例的父类部分被正确地初始化）

```python
# ，实例化时不会报错
>>> class Demo():
...     def __init__(self):
...         return None
...
>>> d = Demo()  # 可以返回 None 值，系统不报错

>>> class Demo():
...     def __init__(self):
...         return
...
>>> d = Demo()  # 可以直接 return，系统不报错

>>> class Demo():
...     def __init__(self):
...         return 0
...
>>> d = Demo()  # 不能返回非 None 的值，系统报 TypeError
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: __init__() should return None, not 'int'
```

## \_\_del\_\_

`object.__del__(self)` 方法用于实例销毁前的清理工作，使用时需注意：

- 该魔法方法会在实例被销毁时调用
- 如果父类含有该魔法方法，则子类的该魔法方法中必须显式地调用父类的该魔法方法（以确保实例的父类部分正确进行了销毁操作）

> 参考来源：  
> https://docs.python.org/3/reference/datamodel.html#special-method-names
