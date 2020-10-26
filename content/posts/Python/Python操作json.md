---
title: 'Python 操作 json'
date: 2018-12-23T19:14:18+08:00
lastmod: 2020-10-26T11:17:21+08:00
tags: ['Python']
categories: ['Notes']
authors:
  - '潘峰'
---

### 导入模块：

`import json`

### 将 Python 对象编码成 JSON 字符串：

`json.dumps()`

```bash
>>> import json
>>> json.dumps({'a': '你好', 'b': 7}, sort_keys=True, indent=4, separators=(',', ': '))
{
    "a": "你好",
    "b": 7
}
```

### 将已编码的 JSON 字符串解码为 Python 对象：

`json.loads()`

```bash
>>> import json
>>> jsonData = '{"a":1,"b":2,"c":3,"d":4,"e":5}'
>>> json.loads(jsonData)
{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
```

### python 原始类型向 json 类型的转化对照表：

| Python           | JSON   |
| ---------------- | ------ |
| dict             | object |
| list, tuple      | array  |
| str, unicode     | string |
| int, long, float | number |
| True             | true   |
| False            | false  |
| None             | null   |

### json 类型转换到 python 的类型对照表：

| JSON          | Python    |
| ------------- | --------- |
| object        | dict      |
| array         | list      |
| string        | unicode   |
| number (int)  | int, long |
| number (real) | float     |
| true          | True      |
| false         | False     |
| null          | None      |

### 中文编码问题

需要注意的是 `json.dumps()` 序列化时对中文默认使用的是 `ascii` 编码，返回时会返回类似于 `"\u4f60\u597d"` 的 `unicode` 字符，因此对于包含中文字符的 json 我们需要借用 `ensure_ascii` 参数进行处理。

- Python 3.x

```bash
>>> import json
>>> js = {"a":"你好"}
>>> json.dumps(js)
'{"a": "\\u4f60\\u597d"}'
>>> json.dumps(js, ensure_ascii=False)
'{"a": "你好"}'
```

- python 2.7
  > 对于 python 2.7，我们可以通过引入 **future** 模块，把新版本的特性导入到当前版本

```bash
>>> import json
>>> from __future__ import unicode_literals
>>> js = {"a":"你好"}
>>> print(json.dumps(js, ensure_ascii=False))
{"a": "你好"}
```
