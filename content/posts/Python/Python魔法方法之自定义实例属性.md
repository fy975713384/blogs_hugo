---
title: 'Python 魔法方法之自定义实例属性'
date: 2020-06-17T12:39:01+08:00
lastmod: 2020-09-15T18:31:35+08:00
tags: ['Python', 'Python魔法方法']
categories: ['Notes']
authors:
  - '潘峰'
---

# Python 魔法方法之自定义实例属性

> _Python 魔法方法系列 让你的代码更加 pythonic_

## \_\_getattribute\_\_()

`object.__getattribute__(self, name)` 方法用于自定义实例属性的访问，使用时需注意：

- 该魔法方法会在访问实例的任何属性时无条件调用。
- 重写该魔法方法时需要返回一个属性值或是触发 `AttributeError` 异常。
- 当定制类继承 `Object` 时，同时也会继承该魔法方法，但它没有执行任何操作。

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

## \_\_getattr\_\_()

`object.__getattr__(self, name)` 方法同样用于自定义实例属性的访问，使用时需注意：

- 该魔法方法会在默认属性访问失败触发 `AttributeError` 时被调用。
  - 该魔法方法因参数 `name` 不是实例属性而触发 `AttributeError`
  - 该魔法方法因参数 `name` 不是类树中用于 `self` 的属性而触发 `AttributeError`
  - 因 `name.__get__()` 触发 `AttributeError`
- 重写该方法时需要返回一个属性值或是触发 `AttributeError` 异常。
- 当定制类继承 `Object` 时，同时也会继承 `object.__getattribute__(self, name)`，但它没有执行任何操作。

## \_\_setattr\_\_()

`object.__setattr__(self, name, value)` 方法用于自定义实例属性的赋值，使用时需注意：

- 该魔法方法会在尝试给实例属性赋值时被调用。
- 如果需要给实例属性赋值，则还需要调用基类的同名方法。

示例代码：

```python
class Karen:
    age = 30
    name = 'Karen'

    def __setattr__(self, key, value):
        print('__setattr__')
        if key == 'age':
            print('age cannot be modified')
        else:
            print('assign value via Object.__setattr__(self, key, value)')
            super().__setattr__(key, value)


p = Karen()
p.age = 'Any'
p.name = 'Karen1'
print(p.age)
print(p.name)
```

执行结果：

```python
__setattr__
age cannot be modified
__setattr__
assign value via Object.__setattr__(self, key, value)
30
Karen1
```

## \_\_delattr\_\_()

`object.__delattr__(self, name)` 方法用于自定义实例属性的删除，使用时需注意：

- 该魔法方法会在 `del obj.name` 或 `delattr(obj, name)` 时被调用。
- 该方法仅当上述动作有意义时才应被实现。

示例代码：

```python
class Karen:
    Katherine = True
    Holdon = True
    Karen = True

    def __delattr__(self, name):
        print('__delattr__')


p = Karen()
del p.Katherine
delattr(p, 'Katherine')
```

执行结果：

```python
__delattr__
__delattr__
```

## \_\_dir\_\_()

`object.__dir__(self)` 在对实例调用 `dir()` 时被调用。该魔法方法必须返回一个 `sequence`。`dir()` 会将该 `sequence` 转换成列表并进行排序。

> 参考来源：  
> https://docs.python.org/3/reference/datamodel.html#special-method-names
