---
title: 'Python 字符串格式化总结'
date: 2018-10-06T16:14:18+08:00
lastmod: 2018-10-06T16:14:18+08:00
tags: ['Python']
categories: ['Notes']
authors:
  - '潘峰'
---

> 推荐使用 f-Strings

## 字符串格式化之一：`%-formatting` 和 `string.Template`

> 转化说明符表达式：`%[转换标记][宽度[.精确度]]转换类型`

| 转换标记 | 解释                                                          |
| -------- | ------------------------------------------------------------- |
| -        | 表示左对齐                                                    |
| +        | 在正数前面显示加号 (+)                                        |
| \*       | 定义宽度或者小数点精度                                        |
| #        | 在八进制数前面显示零 ('0')，在十六进制前面显示 '0x' 或者 '0X' |
| %        | 输出一个单一的 '%'                                            |
| 0        | 显示的数字前面填充 '0' 而不是默认的空格                       |
| <sp>     | 在正数前面显示空格                                            |
| (var)    | 映射变量(字典参数)                                            |

| 转换类型 | 解释                                                      |
| -------- | --------------------------------------------------------- |
| c        | 转换为单个字符，对于数字将转换为该值所对应的 ASCII 码     |
| s        | 转换为字符串，对于非字符串，将默认调用 str() 函数进行转换 |
| r        | 用 repr() 函数进行字符串转换                              |
| i d      | 转换为带符号的十进制数                                    |
| u        | 转换为不带符号的十进制数                                  |
| o        | 转换为不带符号的八进制数                                  |
| x X      | 转换为不带符号十六进制数                                  |
| e E      | 转换为科学计数法表示的浮点数                              |
| f F      | 转换为浮点数（小数部分自然截断）                          |

### 常见用法：

#### 直接格式化字符或者数值

```python
>>> print("Your height is %.2f m" % (1.72))
your height is 1.72 m
```

#### 以元组的形式格式化

```python
>>> itemtuple = ('panfeng', 24, 1.72)
>>> print("Name:%-13s Age:%-8d Height:%-3.2f" % itemtuple)
Name:panfeng       Age:24       Height:1.72
```

#### 以字典的形式格式化

```python
>>> itemdic = {"name": "panfeng", "age": 24, "height": 1.72}
>>> print("Name:%(name)-13s Age:%(age)-8d Height:%(height)-3.2f" % itemdic)
Name:panfeng       Age:24       Height:1.72
```

### string.Template

```python
>>> import string
>>> st = string.Template("Name:$name   Age:$age   Height:$height")
>>> itemdict = {"name": "panfeng", "age": 24, "height": 1.72}
>>> st.substitute(itemdict)
'Name:panfeng   Age:24   Height:1.72'
```

## 字符串格式化之二：`str.format()`

> 表达式：`"{[replacement_field]}".format()`

```text
# 替换字段的语法如下：
replacement_field  ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"
field_name         ::=  arg_name ("." attribute_name | "[" element_index "]")*
arg_name           ::=  [identifier | digit+]
attribute_name     ::=  identifier
element_index      ::=  digit+ | index_string
index_string       ::=  <any source character except "]"> +
conversion         ::=  "r" | "s" | "a"
format_spec        ::=  <described in the next section>
```

```text
# 标准格式说明符的一般形式是：
format_spec     ::=  [[fill]align][sign][#][0][width][grouping_option][.precision][type]
fill            ::=  <any character>
align           ::=  "<" | ">" | "=" | "^"
sign            ::=  "+" | "-" | " "
width           ::=  digit+
grouping_option ::=  "_" | ","
precision       ::=  digit+
type            ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%"
```

### 常见用法

###### 不指定位置，按默认顺序

```python
>>> "{}{}{}".format("No1", "No2", "No3")
'No1No2No3'
```

###### 设置指定位置

```python
>>> "{0}{1}{0}".format("No1", "No2", "No3")
'No1No2No1'
```

###### 直接设置参数

```python
>>> "{thi}{sec}{fir}".format(fir="No1", sec="No2", thi="No3")
'No3No2No1'
```

###### 通过列表索引设置参数

```python
>>> itemlist = ["No1", "No2", "No3"]
>>> "{0[0]}{0[1]}{0[2]}".format(itemlist)
'No1No2No3'
```

###### 通过字典设置参数

```python
>>> itemdict = {"fir": "No1", "sec": "No2", "thi": "No3"}
>>> "{fir}{sec}{thi}".format(**itemdict)
'No1No2No3'
```

## 字符串格式化之三：`f-Strings`

> 表达式：`f' <text> {<expression><optional !s, !r, or !a><optional:format specifier>} <text> ...'`

处理 f-string 中的花括号：

```python
>>> fs = f'{{}}'
'{}'
```
