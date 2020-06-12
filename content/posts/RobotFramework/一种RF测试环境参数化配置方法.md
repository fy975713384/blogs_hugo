---
title: '一种RF测试环境参数化配置方法'
date: 2019-08-26T15:06:49+08:00
lastmod: 2020-08-26T15:06:49+08:00
tags: ['RobotFramework', 'RF进阶']
categories: ['Notes']
authors:
  - 'wing'
---

> 自动化测试脚本通常都需要在不同环境中运行，良好的测试环境参数化配置会使脚本的运行具备较高的灵活性。
> Robot Framework 框架支持一种「嵌套变量」的特性，我们可以利用该特性完成一种优雅的环境参数化配置。

```yaml
*** Variables ***
# 环境管理
${environment}    staging

# 域名管理
${host_api}               ${host_api_${environment}}
${host_api_test}          https://api-test.xxx.com
${host_api_staging}       https://api-staging.xxx.com

${host_api-other}               ${host_api-other_${environment}}
${host_api-other_test}          https://api-other-test.xxx.com
${host_api-other_staging}       https://api-other-staging.xxx.com
```
