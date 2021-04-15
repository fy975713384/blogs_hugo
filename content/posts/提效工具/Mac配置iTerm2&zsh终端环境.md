---
title: 'Mac 配置 iTerm2 & zsh 终端环境'
date: 2020-06-19T23:01:23+08:00
lastmod: 2021-04-15T10:04:52+08:00
tags: ['提效工具', 'iterm2', 'zsh']
categories: ['Notes']
authors:
  - '潘峰'
---

> 最近很悲催，工作用的 2019 款 13“ MacBookPro 主板坏了（据网管说已经坏了好几个了），而且一时大意没有用 Time Machine 做备份（非工作用的倒是做了 🤪🤪🤪），所以趁机水一篇吧。。  
> 所幸的是重要的文件都用 iCloud 进行了同步（下载好慢 🤪🤪🤪），JetBrains 全家桶和 VSCode 都通过 GitHub gist 进行了同步，所以仅需要手动配置终端环境就 OK 了。

> 注：本文大部分配置过程可能需要自行准备梯子，否则遇到连接失败的问题时请自行百度相关工具的国内安装方法

- **Homebrew**: _Mac 下最好用的包管理器_
- **iTerm2**: _Mac 下最好用的终端工具_
- **zsh**: _Mac 下最好用的终端_
- **Oh My Zsh**: _最好用的 zsh 配置管理工具_

## First. 使用 Mac 自带终端安装 Homebrew

[Homebrew 官网](https://brew.sh/) 【_The Missing Package Manager for macOS (or Linux)_】

安装命令（老规矩 `$` 是命令行提示符，执行时不需要输入）：

```shell
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

> githubusercontent.com 域名我曾一度即使有梯子也因为没有配置终端代理导致命令执行不成功，索性直接通过浏览器打开网址将其作为文件下载下来然后通过执行 `/bin/bash -c 文件名` 来完成了安装  
> 直接下载下来的 install.sh 文件可能没有执行权限，需要执行 `chmod +x 文件名` 命令增加执行权限

## Second. 使用 Homebrew 安装 iTerm2 并进行基础配置

[iterm2 官网](https://www.iterm2.com/) 【_iTerm2 - macOS Terminal Replacement_】

1. 安装

   ```shell
   $ brew install iterm2 --cask
   ```

2. 配置 iTerm2 基础设置

   设置 iTerm2 为默认终端  
   打开并切换到 iTerm2 >> (Mac 左上角菜单栏）iTerm2 >> Make iTerm2 Default Term

   设置 iTerm2 全局热键  
   切换到 iTerm2 >> 打开 Preferences（快捷键为 `Command + ,`） >> Keys >> Hotkey >> 勾选 Show/hide iTerm2 with a system-wide hotkey >> 设置 Hotkey 为 `Command + .`  
   _这样便可通过全局热键 `Command + .` 来打开或关闭 iTerm2 窗口_

## Third. 个性化配置 iTerm2 让你的终端工具更加动人

1. 下载我最喜欢的 Ayu 主题：[Ayu Mirage](https://github.com/michelegera/iterm2-ayu-mirage)，如何在 iTerm2 进行配置链接中也有说明不再赘述。  
   喜欢其他主题的如 [Solarized](https://ethanschoonover.com/solarized/)，也可按同样的方法自行配置。

2. 根据自己的喜好配置 Preferences >> Appearance

3. 更换字体

   Preferences >> Profiles >> Text >> Font >> 选择一个自己喜欢的字体  
   这里我用的是我最喜欢的 [Roboto Mono](https://fonts.google.com/specimen/Roboto+Mono)，粗细为 `Bold`，大小为 `12`  
   【_强烈建议选用任意一种 [等宽字体](https://baike.baidu.com/item/%E7%AD%89%E5%AE%BD%E5%AD%97%E4%BD%93)_】

4. Profils 配置完成后导出备份

   Profiles 页左下角 Other Actions... >> Save Profile As Json...  
   建议保存在 iCloud 中进行同步

## Fourth. 使用 iTerm2 安装 Oh My Zsh

> zsh 是 Mac 自带，无需安装，仅安装其配置工具 Oh My Zsh 即可

[Oh My Zsh](https://ohmyz.sh/) 【_Your terminal never felt this good before_】

安装命令：

```shell
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Fifth. 个性化配置 zsh 让你的终端更加美观

使用 iTerm2 打开 zsh 的配置文件 .zsh：

```shell
$ vim ~/.zsh
```

然后找到 `ZSH_THEME` 这个配置项然后将其修改为我最喜欢的一个主题 `ZSH_THEME="ys"`，也可自行知乎安装并设置为其它自己喜欢的主题（仅需要把 `ys` 换成喜欢的那个主题名即可，无需任何下载安装）。

## Sixth. 使用 Homebrew 安装 zsh 插件

推荐安装两个插件：`zsh-autosuggestions` 和 `zsh-syntax-highlighting`

```shell
$ brew install zsh-autosuggestions zsh-syntax-highlighting
```

安装完成后在 .zshrc 中添加以下两行

```vim
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

修改完成后执行

```shell
$ source ~/.zshrc
```

同时配置 vim 语法高亮

```shell
$ vim ~/.vimrc  # 没有该文件的话会同时创建
```

然后添加一行

```shell
syntax on
```

保存便完成了 vim 语法高亮显示

对其它插件感兴趣的话可以用 Homebrew 搜索一下

```shell
$ brew search zsh-
```

## Additional. 配置终端网络代理加速你的 Homebrew

> 由于某些原因 Homebrew 在国内下载速度十分慢，试过很多方法都不太理想（如配置各种国内源），只有配置终端网络代理体验极佳（仅仅需要有梯子）

1. 打开代理工具，查看代理服务器设置中的 HTTP 端口号

2. 使用 iTerm2 打开 zsh 的配置文件 .zsh

   ```shell
   $ vim ~/.zsh
   ```

3. 在文件最底部添加如下两行并保存

   ```shell
   alias proxy="export ALL_PROXY='http://127.0.0.1:xxxx'"
   alias proxyreset="unset ALL_PROXY"
   ```

4. 在 iTerm2 中执行 `proxy` 命令启动代理，执行 `proxyreset` 则可关闭代理

## Final. 命令行工具推荐

- [autojump](https://github.com/wting/autojump)

  可以很方便地在文件夹之间进行跳转，支持通过 Homebrew 安装

- [htop](https://hisham.hm/htop/)

  可以很清晰地查看系统进程，支持通过 Homebrew 安装

> 参考来源：  
> https://www.cnblogs.com/weixuqin/p/7029177.html
