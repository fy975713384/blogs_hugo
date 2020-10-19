---
title: "MacOS 系统长时间使用后变慢"
date: 2020-09-21T11:15:34+08:00
lastmod: 2020-09-21T11:15:34+08:00
tags: ['踩坑日志']
categories: ['Notes']
authors:
  - '潘峰'
draft: true
---

## 背景

问题出现时间：2020 年 9 月

系统环境：macOS 10.15.6（公司统一配备 MacBookPro）

机器配置：2.3 GHz 双核Intel Core i5 + 16 GB 2133 MHz LPDDR3 

问题表现：电脑在不关机情况下使用数天后变得比较卡顿，在使用 iTerm 时会表现的比较明显。

## 排查过程

使用 htop 命令查看系统资源消耗，发现 Swp 有被使用，正常情况下 Swp 