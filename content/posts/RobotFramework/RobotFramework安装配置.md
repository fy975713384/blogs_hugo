---
title: 'Robot Framework 安装配置'
date: 2019-02-19T00:27:04+08:00
lastmod: 2019-02-19T00:27:04+08:00
tags: ['RobotFramework', 'RF入门']
categories: ['Notes']
authors:
  - '潘峰'
---

_RF 是最强的自动化测试框架，没有之一！_

> Robot Framework 最新基于 Windows+Python3 的安装方式，是时候卸载掉 Python2 了！

## Robot Framework 介绍

_Robot Framework 是一款基于 Python 的功能自动化测试框架。它具备良好的可扩展性，支持关键字驱动，可以同时测试多种类型的客户端或者接口，可以进行分布式测试执行。主要用于轮次很多的验收测试和验收测试驱动开发（ATDD）。在我们进行全球化测试的时候可以用此框架来编写一些脚本任务，如定时下载 daily build , 配合 Selenium 完成自动化截图等，来方便我们的测试。_

**`以下使用 <python_path> 代指 python 的本地安装路径`**

## Robot Framework 的安装和配置

### 一、安装 Python

Python 建议安装 3.6 及以上版本，Windows 端注意要将 `<python_path>` 和 `<python_path>\Scripts` 加入环境变量；Mac 端建议[使用 brew 安装 Python](https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&tn=baidu&wd=%E4%BD%BF%E7%94%A8%20brew%20%E5%AE%89%E8%A3%85%20Python&oq=brew%25E5%25AE%2589%25E8%25A3%2585python&rsv_pq=8d4db15d00059a50&rsv_t=5c05WE34UHzDid5UVd7%2BBJWL%2FYI9X%2FimL17iCKjjZmNoOutHI%2FNSt0CiT7M&rqlang=cn&rsv_enter=1&inputT=785&rsv_sug3=44&rsv_sug1=27&rsv_sug7=100&rsv_sug2=0&rsv_sug4=1445)，即可不用考虑环境变量问题。

### 二、安装 RobotFramework 及其所需要的第三方库

_RF 安装及运行所依赖的第三方库均可通过 Python 的包管理器 pip 进行安装。_

1. 安装 Robot Framework：

```bash
$ pip3 install robotframework
```

2. 安装 wxPython：（不安装则无法运行 RIDE 编辑器）

```bash
$ pip3 install wxpython
```

3. 安装 RIDE 编辑器：

- Windows 端可直接使用 pip 安装：

```bash
$ pip3 install robotframework-ride
```

- Mac 端目前需要使用 easy_install 进行安装：

```bash
$ pip3 install -U -r https://raw.githubusercontent.com/robotframework/RIDE/master/requirements.txt
$ git clone https://github.com/robotframework/RIDE.git
$ cd RIDE
$ python3 setup.py build
$ python3 setup.py install
```

4. 其它常用第三方库：

```bash
$ pip3 install robotframework-seleniumlibrary  # 用于进行 Web 自动化测试
$ pip3 install robotframework-appiumlibrary  # 用于进行 app 自动化测试
$ pip3 install robotframework-requests  # 用于进行接口自动化测试
$ pip3 install robotframework-autoitlibrary  # 用于进行 Windows GUI 自动化测试（专用于 Windows 系统，安装时需要管理员权限）
```

### 三、Robot Framework IDE (RIDE) 编辑器的基本使用

RIDE 是官方开发并推荐使用的 RF 测试用例开发环境，完成 RobotFramework 的安装后，Windows 端在命令行中运行 `$ python <python_path>\Scripts\ride.py`，Mac 端直接输入 `$ ride.py` 即可打开 RIDE 编辑器，如图：

![RIDE 编辑器启动界面](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124706.png)

打开 顶部菜单栏 >> Tools，单击 Create RIDE Desktop Shortcut 则可在桌面创建 RIDE 快捷方式，下次即可直接双击快捷方式打开 RIDE 编辑器。
_注意：快捷方式仅支持 Windows 端，Mac 端目前暂不支持。_
![创建快捷方式](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124725.png)

- 创建测试项目 Project
  打开 菜单栏 >> File >> New Project，在弹出的弹窗中输入项目名称，选择 Directory 类型，点击 OK 确认创建；
  ![创建测试项目](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124740.png)

- 创建测试套件 Suite
  右键单击刚创建的测试项目，选择 New Suite，输入套件名称 , 选择 File 类型，点击 OK 确认创建；
  ![创建测试套件](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124801.png)

- 创建测试用例 Case
  右键单击刚创建的测试套件，选择 New Test Case，输入用例名称，点击 OK 确认创建；
  ![创建测试用例](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124819.png)

- 导入 RF 的第三方库（以 SeleniumLibrary 库为例）
  选择刚创建的测试套件，点击最右侧 Library 按钮，在弹出的弹窗中输入库名称，其余可不填，点击 OK 确认导入；
  ![导入 SeleniumLibrary 库](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124841.png)

导入后如果导入的库显示为红色，表示导入的库不存在（检查是否已安装相关的库，拼写是否正确，仍不行的话重启下 RIDE），如果是黑色则表示导入成功；
![导入成功](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124906.png)

- 编写测试脚本：（需要安装过 Chrome 和 对应版本的 chromedriver）
  选择刚创建的测试用例，在 Edit 页的表格中输入脚本；
  ![编写测试脚本](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124926.png)
- 执行测试：
  勾选测试用例，点击运行按钮执行测试；（会正常打开 chrome 并跳转到简书作者首页）
  ![执行测试用例](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124945.png)

- 查看测试报告：
  ![查看测试报告](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119125004.png)

> 参考来源：  
> https://github.com/robotframework/RIDE/releases  
> https://www.ibm.com/developerworks/cn/opensource/os-cn-robot-framework/index.html  
> https://github.com/robotframework/SeleniumLibrary  
> https://github.com/serhatbolsu/robotframework-appiumlibrary  
> https://github.com/nokia/robotframework-autoitlibrary
