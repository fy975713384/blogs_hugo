---
title: 'Python Launcher（for Windows）'
date: 2018-09-09T18:27:30+08:00
lastmod: 2018-09-09T18:27:30+08:00
tags: ['Python']
categories: ['Notes']
authors:
  - '潘峰'
---

[TOC]

# Python Launcher（for Windows）

## 1. 什么是 Python Launcher

**_原文：_**

_New in version 3.3._

_The Python launcher for Windows is a utility which aids in locating and executing of different Python versions. It allows scripts (or the command-line) to indicate a preference for a specific Python version, and will locate and execute that version._

_Unlike the PATH variable, the launcher will correctly select the most appropriate version of Python. It will prefer per-user installations over system-wide ones, and orders by language version rather than using the most recently installed version._

-----来源于 python 官方文档

**翻译：**

3.3 版本新增功能

适用于 Windows 的 Python 启动器是一个实用组件，可帮助您定位和执行不同的 Python 版本。它允许脚本（或命令行）为特定的 Python 版本指示首选项，定位并执行该版本。

不同于 PATH 变量，Python Launcher 将正确选择最合适的 Python 版本。它更喜欢每个用户在系统范围内的安装，并且运行指定的 Python 版本，而不是使用最近安装的版本。

## 2. Python Launcher 的位置

在安装 Python 3.3 以上版本的 Python 时，我们可以看到下图中的一个选项，当我们勾选时，系统便会帮助我们自动安装 Python Launcher

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn61ddmwlj20iw0bqgnw.jpg)

当点击 Customize installation 时，会显示如下界面：

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn68kg4duj20iw0bq40r.jpg)

图中标注的小字告诉我们，安装 Python Launcher 后可以通过全局命令 ‘py’ 来更方便地启动 Python。不勾选 launcher 项时，系统则不会安装 Python Launcher （但默认情况下该工具都是被勾选的）。

安装好后，我们可以通过注册表和系统文件管理器中找到 py.exe 的位置:

- 仅为当前用户安装时：

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn69n6194j20qj0e3ta2.jpg)

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn6a2mkejj20l20ejgmy.jpg)

- 为所有用户安装时：

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn6azwjjvj20qj0e3abf.jpg)
![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn6b9v8bxj20m50fhq56.jpg)

## 3. Python Launcher 的环境变量

为什么 Python Launcher 不需要手动配置环境变量呢？

当为所有用户安装时，我们会发现 py.exe 被自动安装到 C:\Windows 下了，当我们检查系统默认设置的环境变量时，可以看到下图中的 windir 的值已经默认设置为 C:\Windows ，这第一点对于所有 Windows 系统都是相同的 ，系统安装的时候便会做这些工作。

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn6ccrmqpj20ay0badgh.jpg)

当仅为当前用户安装时，我们会发现 py.exe 所在的路径会自动添加到系统的用户变量中：

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn6cwk3vcj20ay0bagmo.jpg)

因此，无论以哪种方式安装 Python Launcher 我们都不需要再手动配置环境变量了。

## 4. Python Launcher 的用法

Python Launcher 的使用方法我们可以通过在命令行中输入 py -h 进行查看，如下图：

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn6dbotnrj20i706bmx0.jpg)

同样地，我们也可以通过查阅官方文档进行了解

[https://docs.python.org/3/using/windows.html#launcher](https://docs.python.org/3/using/windows.html#launcher)

![image](http://ww1.sinaimg.cn/large/ed19fa55gy1gfn6do1wf3j20mn0ih0t8.jpg)

## 5. 结语

综上所述，py -version 启动 python 的方式其实应该是官方更为推崇的一种方式，无需手动设置环境变量，并且能指定启动 python 的版本，功能不要太强大！绝对是在多版本 Python 共存的环境下，启动不同版本 Python 的利器！

当是需要注意的是，Python Launcher 是 Python3.3 以上版本中新增的组件，并且可以独立地安装和卸载，使用时一定要注意 Python Launcher 被正确地安装在系统中了，否则就会报 _'py' 不是内部或外部命令，也不是可运行的程序或批处理文件_ 的错误，一定要注意哦！

好了，本期调研的全部内容到这里就结束了，感谢大家阅览！(づ￣ 3 ￣)づ</br>
我们下期再见！</br>
[好累 ┭┮﹏┭┮ ，不要再有下期了...]
