---
title: 'Python 魔法方法之自定义属性访问'
date: 2020-06-17T12:39:01+08:00
lastmod: 2020-06-17T12:39:01+08:00
tags: ['Python', 'Python魔法方法']
categories: ['Notes']
authors:
  - '潘峰'
draft: true
---

# Python 魔法方法之自定义属性访问

> _Python 魔法方法系列 让你的代码更加 pythonic 一些_

## \_\_getattribute\_\_

- `object.__getattribute__(self, name)` 方法会在访问实例的任何属性时无条件调用。
- 重写该方法时需要返回一个属性值或是触发 `AttributeError` 异常。
- 当定制类继承 `Object` 时，同时也会继承 `object.__getattribute__(self, name)`，但它没有执行任何操作。

示例代码：

```python
class Karen:
    age = '30'

    def __getattribute__(self, name):
        print('__getattribute__')
        return None

    def holdon(self):
        print('I am protector')


p = Karen()
print(p.age)
print(p.sex)
print(p.holdon)
```

执行结果：

```python
__getattribute__
None
__getattribute__
None
__getattribute__
None
```

可以看出，无论是访问 `Karen` 已有的属性 `age` 和方法 `holdon`，还是没有的属性 `sex`，都会执行 `__getattribute__` 方法，并返回该方法的返回值。

当需要其返回属性原有的值的时候，可以返回基类 (Object) 的 `__getattribute__` 方法并传递相同的 `name` 参数：

```python
...
    def __getattribute__(self, name):
        print('__getattribute__')
        return super().__getattribute__(name)
...


p = Karen()
print(p.age)
p.holdon()
```

执行结果：

```python
__getattribute__
30
__getattribute__
I am protector
```

另外要注意避免自定义该方法时可能产生的无限递归：

- 示例一

  ```python
  ...
      def __getattribute__(self, name):
          print('__getattribute__')
          if name == "sex":
              return 'w'
          else:
              return self.holdon()
  ...

  p = Karen()
  p.protector
  ```

  执行结果：

  ```python
  RecursionError: maximum recursion depth exceeded while calling a Python object
  ```

- 示例二

  ```python
  ...
      def __getattribute__(self, name):
          print('__getattribute__')
          if name == "holdon":
              return self.holdon()
          return super().__getattribute__(name)
  ...

  p = Karen()
  p.holdon
  ```

  执行结果：

  ```python
  __getattribute__
  30
  __getattribute__
  ...
  RecursionError: maximum recursion depth exceeded while calling a Python object
  ```

## \_\_getattr\_\_

- `object.__getattr__(self, name)` 方法会在默认属性访问失败触发 `AttributeError` 时被调用。
  - `object.__getattribute__(self, name)` 因 `name` 不是实例属性而触发 `AttributeError`
  - 因 `name` 不是类树中用于 `self` 的属性而触发 `AttributeError`
  - `name.__get__()` 触发 `AttributeError`
- 重写该方法时需要返回一个属性值或是触发 `AttributeError` 异常。
- 当定制类继承 `Object` 时，同时也会继承 `object.__getattribute__(self, name)`，但它没有执行任何操作。

> 参考来源：  
> https://docs.python.org/3/reference/datamodel.html#special-method-names
