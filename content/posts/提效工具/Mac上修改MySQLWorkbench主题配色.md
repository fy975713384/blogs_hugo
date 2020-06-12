---
title: 'Mac 上修改 MySQL Workbench 主题配色'
date: 2019-09-30T15:52:42+08:00
lastmod: 2019-09-30T15:52:42+08:00
tags: ['提效工具']
categories: ['Notes']
authors:
  - 'wing'
---

> 在 Mac 上使用了 Mojava 的深色主题，当我安装 Mysql Workbench 时，它也默认使用了深色主题的配色，但 Mysql Workbench 的深色主题让我十分不适应，因此想改为浅色主题，但发现设置中根本没有和主题相关的配置，Google 了好久终于找到了一种方法

- 从深色主题修改为浅色主题：（在命令行中执行以下命令）

```shell
$ defaults write com.oracle.workbench.MySQLWorkbench NSRequiresAquaSystemAppearance -bool yes
```

- 从浅色主题恢复为深色主题：（仅需将上条命令中的 yes 改为 no 然后重新执行一遍命令）

```shell
$ defaults write com.oracle.workbench.MySQLWorkbench NSRequiresAquaSystemAppearance -bool no
```

重新打开 MySQL Workbench 客户端即可生效

> 参考来源：  
> [https://stackoverflow.com/questions/17325408/mysql-workbench-dark-theme](https://stackoverflow.com/questions/17325408/mysql-workbench-dark-theme)
