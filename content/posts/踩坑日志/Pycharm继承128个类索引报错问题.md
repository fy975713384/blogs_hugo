---
title: 'Pycharm 继承类过多时创建索引失败问题'
date: 2020-08-21T03:59:04+08:00
lastmod: 2020-08-26T14:39:52+08:00
tags: ['踩坑日志']
categories: ['Notes']
authors:
  - '潘峰'
draft: true
---

> 最近团队在使用 Pycharm 的过程中一直被一个问题所困扰，就是通过 Pycharm 打开自动化测试项目时，Pycharm 一直显示在 indexing，导致工具的相关功能无法使用，且会占用大量 Mac 机器的资源，导致无法进行其它工作。  
> 因为该问题，一些同事无奈选择了更换其它工具进行开发，或者是通过一些方法让 Pycharm 不进行索引（不索引的话也会导致高亮显示、自动补全和方法跳转等功能无法使用）。  
> 作为 Pycharm 重度依赖用户，以及对该问题的强烈好奇，我决定硬着头皮死磕。。。

### 背景

问题暴露时间：2020 年 4~5 月

Pycharm 版本：PyCharm 2020.1.3 (Professional Edition)

系统环境：macOS 10.15.6（公司统一配备 MacBookPro）

### 小赢自动化测试项目结构

小赢的自动化测试项目代码总体采用了一种菱形继承的结构，在中间一层写了大量的业务关键字库，由最顶层的库继承去执行测试。

```mermaid
graph TB
    subgraph 菱形继承
        LibAsset --> |继承| BusBase
        ... -->  |继承| BusBase
        LibFund --> |继承| BusBase
        LoanLibrary --> |继承| LibAsset
        LoanLibrary --> |继承| ...
        LoanLibrary --> |继承| LibFund
    end
```

最顶层 `LoanLibrary` 代码大概如下

```python
class LoanLibrary(
    Libxxx,
    Libxxx,
    LibCxxx,
    ...
  ):
  pass
```

### 排查过程

在我本地遇到该问题的时候，并没有同事反馈该问题，我推测可能是我本地代码或工具的原因。
因此我先查看了下本地 Pycharm 日志，发现日志每次索引完成后日志都会打印一段包含了该信息 `Caused by: java.io.IOException: Unknown custom stub type Libxxx` 的报错然后再次开始索引。
于是我便在谷歌了一下相关问题，得到的答案是通过 Pycharm 的 `invalidate caches/restart...` 功能可以解决，但多次尝试后，报错依然发生。无奈我又尝试替换 Pycharm 版本，问题仍旧存在。
实验无果后，我便试图查看 `Libxxx` 这个自定义库中是否有长得较为诡异的代码，几经修改后，问题还是没有解决。
但在尝试过程中，我发现在打开最顶层的 `LoanLibrary` 库时日志报错会激增，然后关闭项目重新打开后，索引就能正常建完且不在报错了。
当这种操作可以临时解决我的问题时，我便先暂停了问题的后续排查。

大概一个星期后，陆续有同事开始反应遇到了同样的问题，这时我意识到问题可能来源于自动化测试项目本身，而和系统版本、工具版本无关。

由于问题是在 4~5 月前后才开始大量出现，我推测应该是由于某个同事的提交导致的，但这之间有近百个提交，于是我采用了二分法查找的原理去 `checkout` 各个提交版本来进行定位，最终确实定位到了某个离职同事的提交，令我心头一喜。
通过 review 该同事的提交，并没有发现一些异常的代码，仅仅是新增了一些业务自动化关键字库，于是且切回最新的分支后，我手动去除了他的提交，本以为这样就没问题了，但结果让我心如死灰。。。

虽然结果不如人意，但在不断尝试的过程中，我意识到问题确实出在最顶层的 `LoanLibrary` 库中，我尝试着注释掉 `LoanLibrary` 中自出问题的那个版本后的全部提交然后再试一次，于是现实再一次给我泼了一盆冷水。。。
仔细思考后，直觉告诉我，可能除了信贷的 `LoanLibrary`，支付的 `PayLibrary` 也会导致该问题，因为 `PayLibrary` 同样继承了太多的关键字库，也许是某次提交触发了类似的问题。。。
这一次，实验结果令人振奋，在注释了 `PayLibrary` 5 月份之后新增的关键字库后，报错停止了，索引也都正常地创建完成。

但问题的排查并没有结束，我开始尝试按时间顺序将最早提交的新增关键字库一个个取消注释，最终 `LoanLibrary` 和 `PayLibrary` 仅剩极少的新增关键字库被注释掉了。
在反复 review 剩下的关键字库的新增代码后，我并未发现任何异常，在凝视了 `LoanLibrary` 和 `PayLibrary` 两个类很久后，一个大胆的想法出现在我的脑中：难道和继承的类的数量有关吗？
于是我数量一下目前两个类继承的类的数量，答案令人激动，数据都是 127 个！破案了！

### 问题的具体表现

在菱形继承的结构下，如果最上层类继承的类大于等于 128 个时，Pycharm 创建索引会一直失败，导致不停地索引。

通过以下代码创建一个 Python 项目（pycharm_bug），在 PyCharm 2020.1.3 及其更早的版本（不限社区版和专业版）打开，可以复现该问题。

```python
import os

os.mkdir('pycharm_bug')

with open('pycharm_bug/class_base.py', 'a') as f:
    f.write('class BaseClass: pass')

with open('pycharm_bug/class_derivative.py', 'a') as f:
    f.write('from class_base import BaseClass\n\n\n')
    for i in range(129):
        f.write(f'class TestDemo{i}(BaseClass): pass\n')

with open('pycharm_bug/class_top.py', 'a') as f:
    f.write('from class_derivative import *\n\n\n')
    f.write('class ClassTop(')
    for i in range(129):
        f.write(f'TestDemo{i}, ')
    f.write('): pass')
```

### 后续

团队讨论后决定在 `LoanLibrary` 等继承过多类的库下方再多增加一层，使顶层不至于继承太多的类，这样使问题暂时得到解决，小伙伴们又可以愉快地使用 Pycharm 了。

之后，团队中英语最好的同事给 Pycharm 官方提了一个 BUG：
![pycharmbug.jpg](http://ww1.sinaimg.cn/large/ed19fa55gy1gi47an7451j218i10d43a.jpg)
