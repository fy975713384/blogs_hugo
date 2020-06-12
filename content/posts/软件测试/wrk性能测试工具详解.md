---
title: 'wrk 性能测试工具详解'
date: 2019-11-12T16:52:49+08:00
lastmod: 2019-11-12T16:52:49+08:00
tags: ['性能测试', '提效工具']
categories: ['Notes']
authors:
  - 'wing'
---

## wrk 简介

wrk 是一个用于 HTTP 协议的基准测试工具。

> 基准测试是一种性能测试方法，它通过设计科学的测试方法、测试工具和测试系统，实现对一类测试对象的某项性能指标进行定量和可对比的测试。

GitHub 地址：[https://github.com/wg/wrk](https://github.com/wg/wrk)

## 安装

Mac 直接使用 HomeBrew 进行安装

```bash
$ brew install wrk
```

## 命令行选项

在命令行中输入 `wrk` 查看命令行选项：

```bash
$ wrk
Usage: wrk <options> <url>
  Options:
    -c, --connections <N>  Connections to keep open
                            # 设置总的 http 并发连接数
    -d, --duration    <T>  Duration of test
                            # 设置测试持续时间
    -t, --threads     <N>  Number of threads to use
                            # 设置开启的线程数（不能大于设置的并发请求数）
    -s, --script      <S>  Load Lua script file
                            # 加载指定的 Lua 脚本
    -H, --header      <H>  Add header to request
                            # 添加 http 请求头
        --latency          Print latency statistics
                            # 打印延迟统计情况（建议启用）
        --timeout     <T>  Socket/request timeout
    -v, --version          Print version details

  Numeric arguments may include a SI unit (1k, 1M, 1G)
  Time arguments may include a time unit (2s, 2m, 2h)
```

## 基础示例

GET 请求：

```shell
$ wrk -c100 -t10 -d10s --latency http://localhost:5000/test_get
# 100 个请求分 10 个线程压 10 秒
```

控制台输出：

```shell
Running 10s test @ http://localhost:5000/test_get
  10 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   201.71ms   58.06ms 451.62ms   79.16%
    Req/Sec    27.94     10.18    70.00     73.70%
  Latency Distribution
     50%  196.82ms
     75%  228.07ms
     90%  261.79ms
     99%  394.08ms
  2817 requests in 10.10s, 473.18KB read
Requests/sec:    278.94
Transfer/sec:     46.86KB
```

---

POST 请求：

```shell
$ wrk -c100 -t10 -d10s -s test_post.lua --latency http://localhost:5000/test_post
# 使用 test_post.lua 脚本
```

Lua 脚本：

```lua
wrk.method = "POST"
wrk.headers["Content-Type"] = "application/json"
wrk.body = '{"k1":1,"k2":2}'
```

控制台输出：

```bash
Running 10s test @ http://localhost:5000/test_post
  10 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   188.49ms   60.76ms 352.02ms   67.50%
    Req/Sec    38.35     23.71   101.00     67.22%
  Latency Distribution
     50%  182.22ms
     75%  230.92ms
     90%  272.66ms
     99%  325.02ms
  3665 requests in 10.10s, 604.87KB read
Requests/sec:    362.77
Transfer/sec:     59.87KB
```

注：GET 请求同样可以使用 Lua 脚本

## 进阶用法

### Lua 脚本在压测时的全局变量 (tabel) wrk

#### 全局变量

```lua
wrk = {
    scheme  = "http",
    host    = "localhost",
    port    = nil,
    method  = "GET",
    path    = "/",
    headers = {},
    body    = nil,
    thread  = <userdata>,
}
```

#### 全局方法

```lua
function wrk.format(method, path, headers, body)
--[[
  wrk.format 方法返回一个 HTTP 请求的字符串，包含传入参数与 wrk 变量的值的组合。
]]--

function wrk.lookup(host, service)
--[[
  wrk.lookup 方法返回一个包含主机所有已知地址的 table 和服务对，这对应于 POSIX 的 getaddrinfo() 方法
]]--
-- host:一个主机名或者地址串(IPv4的点分十进制串或者IPv6的16进制串)
-- service：服务名可以是十进制的端口号，也可以是已定义的服务名称，如ftp、http等

function wrk.connect(addr)
--[[
  判断 addr 是否可以连接上，返回布尔值。addr 必须是一个 wrk.lookup() 方法的返回值。
]]--
```

### Lua 脚本的生命周期钩子

#### 启动阶段 (Setup)

> 启动阶段开始于请求目标 IP 地址已解析且所有线程已经初始化但还没启动的时候

```lua
function setup(thread)
--[[
  在脚本文件中实现 setup 方法，wrk 就会在目标 IP 已解析且测试线程初始化完成但还没有启动的时候调用该方法。

  wrk 会为每一个测试线程调用一次 setup 方法，并传入代表测试线程的对象 thread 作为参数。
  setup 方法中可操作该 thread 对象，获取信息、存储信息、甚至关闭该线程。
  setup 方法每个线程只执行一次。

  thread 对象提供了 1 个属性，3 个方法：
    thread.addr 获取或设置线程的服务器地址
    thread:get(name) 获取线程全局变量
    thread:set(name, value) 设置线程全局变量
    thread:stop() 终止线程
]]--
```

#### 运行阶段 (Running)

> 运行阶段开始于对 init 方法的单次调用，让后在每个请求周期都调用一次 request 和 response 方法

```lua
function init(args)
-- 每个线程仅调用 1 次，args 用于获取当前脚本命令行中所有传入的其它命令参数，例如 --env=pre

function delay()
-- 每次发起请求前调用 1 次，在 request 方法前调用，单位为 ms

function request()
-- 每次发起请求前调用 1 次，返回一个包含 HTTP 请求的字符串，wrk 会按该字符串的内容发起 HTTP 请求。
-- 通常会在该方法中调用 wrk.format() 生成 HTTP 请求字符串。

function response(status, headers, body)
-- 每次请求返回时调用 1 次，wrk 通过传入 HTTP 响应码，响应头，响应体调用。
```

#### 结束阶段 (Done)

```lua
function done(summary, latency, requests)
--[[
  done 方法接收一个包含测试结果数据的 table 变量 summary 以及两个分别表示每个请求的延迟时间和每个线程的 QPS 的统计对象。持续时间和延迟时间单位均为 ms。

  整个测试过程中仅被调用 1 次。

  summary = {
    duration = N,  -- 运行持续时间，单位为 ms
    requests = N,  -- 总共完成的请求书
    bytes    = N,  -- 总共接收的数据量
    errors   = {
      connect = N, -- socket 连接总失败数
      read    = N, -- socket 连接读取总失败数
      write   = N, -- socket 连接写入总失败数
      status  = N, -- total HTTP status codes > 399
      timeout = N  -- total request timeouts
    }
  }

  latency.min              -- 最小延时
  latency.max              -- 最大延时
  latency.mean             -- 平均延时
  latency.stdev            -- 延时标准差
  latency:percentile(99.0) -- 99th percentile value
  latency(i)               -- raw value and count
]]--
```

#### 调用过程示例

命令行语句：

```bash
$ wrk -c1 -t1 -d1s -s test_lua.lua http://localhost:5000
```

lua 脚本：

```lua
#!/usr/local/bin/lua

wrk.body = '{"k1":666}'
path = "/test_post"

function setup(thread)
    print("调用 setup()")
    print(thread.addr)
end

function init(args)
    print("调用 init()")
end

function delay()
    print("调用 delay() 等待 500 ms")
    return 500
end

function request()
    print("调用 resquest()")
    return wrk.format("POST", path)
end

function response(status, headers, body)
    print("调用 response()")
    if (status == 200)
    then
        path = "/test_get?k1=" .. body
    end
end

function done(summary, latency, requests)
    print("调用 done()")
end
```

控制台输出：

```shell
调用 setup()
127.0.0.1:5000
调用 init()
调用 resquest()
Running 1s test @ http://localhost:5000
  1 threads and 1 connections
调用 delay() 等待 500 ms
调用 resquest()
调用 response()
调用 delay() 等待 500 ms
调用 resquest()
调用 response()
调用 delay() 等待 500 ms
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     1.08ms  135.06us   1.17ms  100.00%
    Req/Sec     1.50      0.71     2.00    100.00%
  2 requests in 1.01s, 525.00B read
  Non-2xx or 3xx responses: 1
Requests/sec:      1.98
Transfer/sec:     519.09B
调用 done()
```

HTTP 请求日志：

```shell
127.0.0.1 - - [15/Nov/2019 11:10:02] "POST /test_post HTTP/1.1" 200 -
127.0.0.1 - - [15/Nov/2019 11:10:03] "POST /test_get?k1={"k1":666} HTTP/1.1" 405 -
```

更多使用示例参见：[https://github.com/wg/wrk/tree/master/scripts](https://github.com/wg/wrk/tree/master/scripts)

> 参考来源：  
> https://github.com/wg/wrk  
> https://github.com/wg/wrk/blob/master/README.md  
> https://github.com/wg/wrk/blob/master/SCRIPTING  
> https://juejin.im/post/5a59e74f5188257353008fea#heading-10  
> https://www.cnblogs.com/xinzhao/p/6233009.html
