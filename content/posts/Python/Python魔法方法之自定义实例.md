---
title: 'Python 魔法方法之自定义实例'
date: 2020-06-17T12:38:01+08:00
lastmod: 2020-08-05T17:23:34+08:00
tags: ['Python', 'Python魔法方法']
categories: ['Notes']
authors:
  - '潘峰'
---

# Python 魔法方法之自定义实例

> _Python 魔法方法系列 让你的代码更加 pythonic_

## \_\_new\_\_

`object.__new__(cls[, ...])` 方法用于实例的创建，使用时需注意：

- 该魔法方法接收被请求创建该实例的类作为其第一个参数，其余参数将被传递给对象构造函数表达式
- 该魔法方法需要返回一个新的对象（通常是 cls）实例，否则 `__init__()` 方法将不会被调用
- 该魔法方法主要目的是给不可变类型的子类提供自定义实例的创建，以及用于自定义元类（用于创建自定义类）

## \_\_init\_\_

`object.__init__(self[, ...])` 方法用于实例的初始化，使用时需注意：

- 该魔法方法

> 参考来源：  
> https://docs.python.org/3/reference/datamodel.html#special-method-names
