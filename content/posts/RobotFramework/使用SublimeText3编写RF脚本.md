---
title: '使用 Sublime Text 3 编写 RF 脚本'
date: 2018-07-16T14:34:07+08:00
lastmod: 2018-07-16T14:34:07+08:00
tags: ['RobotFramework', 'RF进阶', '提效工具', '编辑器', 'Sublime']
categories: ['Notes']
authors:
  - '潘峰'
---

_RF 是最强的自动化测试框架， 没有之一！_

> 前置条件
>
> 1. 已安装 Python2.7
> 2. 已安装好 Robot Framework 框架和相关 Python2.x 测试库
> 3. 任何安装目录中都不要出现中文

## 一、安装配置 Sublime Text 3

1. 在 Sublime 官网根据自己的系统下载并安装相应版本的 [Sublime Text 3](http://www.sublimetext.com/3)；
2. 根据 [Package Control 官网](https://packagecontrol.io/installation) 的说明安装该插件。

## 二、安装配置 RobotFrameworkAssistant

说明文档：[https://packagecontrol.io/packages/RobotFrameworkAssistant](https://packagecontrol.io/packages/RobotFrameworkAssistant)  
GitHub 地址：[https://github.com/andriyko/sublime-robot-framework-assistant](https://github.com/andriyko/sublime-robot-framework-assistant)

### 1. 安装 RobotFrameworkAssistant

安装过程图示：
![安装 RobotFrameworkAssistant.gif](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/ed19fa55gy1gfphy1v1zeg212w0n9jyd.gif)

### 2. 配置 RobotFrameworkAssistant

2.1 打开 `Robot.sublime-settings` 文件，注意选择 `Setting - User`：
![打开 Robot.sublime-settings.png](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124135.png)

2.2 修改 `Robot.sublime-settings` 文件并保存：
![image.png](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124200.png)
示例代码如下：（代码中的相关路径需要修改为自己机器中的实际路径）

- Windows

```json
/* Robot Framework Assistant User settings for panfeng /
{
  "path_to_python": "C:\\Python27\\python.exe",
  "robot_framework_workspace": "D:\\OneDrive\\TesterScript\\RF",
  "robot_framework_module_search_path": ["C:\\Python27\\Lib\\site-packages"],
  "robot_framework_keyword_argument_format": true
}
```

- Mac

```json
/* Robot Framework Assistant User settings for panfeng /
{
  "path_to_python": "python3",
  "robot_framework_workspace": "/Users/panfeng/workspace/RF",
  "robot_framework_module_search_path": [
    "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages"
  ],
  "robot_framework_keyword_argument_format": true
}
```

注意：和 Python 有关的路径需要设置为 Python 2.x 版本，截止到目前 2018.07.16， Robot Framework Assistant 的 Python3 版本兼容性不是很好。

2.3 安装配置完成后大致效果如下：（主题和字体可能不一样）
![](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124258.png)

2.4 打开 `Robot.sublime-build` 配置文件：
![打开 Robot.sublime-build.png](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124314.png)

2.5 修改 `Robot.sublime-build` 配置文件并保存：
![修改 Robot.sublime-build.png](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124337.png)
示例代码如下：

- Windows

```json
{
  "cmd": ["python", "-m", "robot.run", "-d", "log", "$file"],
  "selector": "source.robot",
  "variants": [
    {
      "name": "Selects the test cases by tag",
      "cmd": ["python", "-m", "robot.run", "--include", "REPLACE_WITH_TAG", "-d", "log", "$file"]
    },

    {
      "name": "Selects the test cases by name",
      "cmd": ["python", "-m", "robot", "--test", "REPLACE_WITH_TEST_NAME", "-d", "log", "$file"]
    }
  ]
}
```

- Mac

```json
{
  "cmd": ["python3", "-m", "robot", "-d", "log", "$file"],
  "selector": "source.robot",
  "variants": [
    {
      "name": "Selects the test cases by tag",
      "cmd": ["python3", "-m", "robot", "--include", "REPLACE_WITH_TAG", "-d", "log", "$file"]
    },

    {
      "name": "Selects the test cases by name",
      "cmd": ["python3", "-m", "robot", "--test", "REPLACE_WITH_TEST_NAME", "-d", "log", "$file"]
    }
  ]
}
```

2.6 配置完成后即可通过 Sublime 的构建系统执行脚本
![使用构建系统执行脚本.gif](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/ed19fa55gy1gfpi36dqzug215w0psjy9.gif)

完成上述操作后，Sublime Text 3 便可以高亮显示 RF 语法，并进行相关层次的构建操作。但还不具备关键字自动补全的功能，想要让 Sublime Text 3 可以进行关键字自动补全，还需要进行 Database 的创建。

###### 4. 创建 Database

创建 Database 的操作十分简单，只需要通过 `导航栏 Preference --> Package Setting --> Robot Framework Assistant --> Create Datebase` 或 `右键打开的 RF 文件 --> Robot Framework --> Datebase --> Create Datebase` 进行创建即可。
![创建 Database 1.png](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124441.png)
![创建 Database 2.png](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124508.png)
创建成功后，左下角应该会显示 `Indexing done with rc: 0` ，也可以通过打开 RobotFrameworkAssistant 插件目录（默认在 `C:\Users\用户名\AppData\Roaming\Sublime Text 3\Packages\RobotFrameworkAssistant\database`）下的 database 查看是否创建成功，如果创建成功则会生成 index 和 scanner 两个文件夹。
![](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124539.png)

最终效果：

![关键字自动补全效果.gif](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119124626.png)

## 个性化 Sublime Text 3 配置，让 Sublime 更加美观（可选）

主题：[ayu](https://github.com/dempfi/ayu)  
字体：[Roboto Mono Medium](https://fonts.google.com/specimen/Roboto+Mono)
_（谷歌字体下载需要 fq）_
