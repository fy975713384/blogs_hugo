---
title: 'Python 处理时间与日期'
date: 2019-03-23T11:46:24+08:00
lastmod: 2020-03-23T11:46:24+08:00
tags: ['Python']
categories: ['Notes']
authors:
  - '潘峰'
---

# Python 时间与日期处理

> 在信贷项目中会频繁地对日期时间进行处理，尤其是账单平移工具会大量地进行时间计算，熟练掌握 Python 的日期时间操作将给这些测试工作带来事半功倍的效果。

## 内置库

### time 模块

#### 官方文档

https://docs.python.org/3/library/time.html#

#### 时间对象转换

![时间转换](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119121632.png)

#### 核心类

`class time.struct_time`

| 索引 | 实例属性  | 含义         | 值                        |
| :--- | :-------- | :----------- | :------------------------ |
| 0    | tm_year   | 年           | eg: 2020                  |
| 1    | tm_mon    | 月           | range [1, 12]             |
| 2    | tm_mday   | 日           | range [1, 31]             |
| 3    | tm_hour   | 小时         | range [0, 23]             |
| 4    | tm_min    | 分钟         | range [0, 59]             |
| 5    | tm_sec    | 秒           | range [0, 61]             |
| 6    | tm_wday   | 周           | range [0, 6]，周一为 0    |
| 7    | tm_yday   | 天           | range [1, 366]            |
| 8    | tm_isdst  | 时令         | 0, 1 或 -1                |
| N/A  | tm_zone   | 时区名称缩写 |                           |
| N/A  | tm_gmtoff | -            | 以秒为单位的 UTC 以东偏离 |

#### 示例代码

- 导入库：`import time`

- 获取当前时间时间戳

```python
t = time.time()

print(t)                       # 原始时间数据
print(int(t))                  # 秒级时间戳
print(int(t * 1000))           # 毫秒级时间戳
print(int(t * 1000000))        # 微秒级时间戳
```

- 获取当前时间的年月日

```python
time.localtime().tm_year  # 年
time.localtime().tm_mon   # 月
time.localtime().tm_mday  # 日
```

- 获取对应时间的格式化字符串

```python
# 当前时间
time.asctime()    # 'Wed Mar  4 17:34:54 2020'
time.ctime()      # 'Wed Mar  4 17:34:54 2020'

# 指定时间
time.asctime(time.localtime(0))    # 'Thu Jan  1 08:00:00 1970'
time.ctime(0)                      # 'Thu Jan  1 08:00:00 1970'
```

- 时间戳转换成时间字符串

```python
timestamp = 1577808000
time_local = time.localtime(timestamp)
time_str = time.strftime('%Y-%m-%d %H:%M:%S', time_local)  # '2020-01-01 00:00:00'
```

- 时间字符串转换成时间戳

```python
time_str = '2020-01-01 00:00:00'
struct_time = time.strptime(time_str, '%Y-%m-%d %H:%M:%S')
timestamp = int(time.mktime(struct_time))  # 1577808000
```

### datetime 模块

https://docs.python.org/3/library/datetime.html#

#### 核心对象

| 对象名    | 类描述                          | 作用                       |
| --------- | ------------------------------- | -------------------------- |
| date      | `class datetime.date(...)`      | 日期操作                   |
| time      | `class datetime.time(...)`      | 时间操作                   |
| datetime  | `class datetime.datetime(...)`  | 约等于 date 和 time 的结合 |
| timedelta | `class datetime.timedelta(...)` | 日期差值计算               |

#### datetime 对象转换

![日期转换](https://cdn.jsdelivr.net/gh/fy975713384/cloud-img@main/blog/20201119121848.png)

#### 示例代码

- 导入库：`from datetime import datetime`

- 获取当前日期和时间：

```python
datetime.now()  # 2017-12-24 21:56:22.698000
```

- 获取当天的年月日

```python
now = datetime.today()
# now = datetime.fromtimestamp(time.time()) 同上

now.year   # 年
now.month  # 月
now.day    # 日
```

- 格式化成需要的时间：

```python
datetime.now().strftime('%Y-%m-%d_%H-%M-%S')  # 2020-02-02_02-02-02
```

- 格式化为 ISO 8601 的日期和时间的组合表示法
  - 方式一：
  ```python
  from datetime import datetime, timezone, timedelta
  datetime.now(tz=timezone(timedelta(hours=8))).isoformat()
  ```
  - 方式二：
  ```python
  import pytz
  datetime.now(tz=pytz.timezone('Asia/Shanghai')).isoformat()
  ```
  - 方式三：
  ```python
  datetime.now().strftime('%Y-%m-%dT%T+08:00')
  ```

### calendar 模块

https://docs.python.org/3/library/calendar.html#

#### 示例代码

- 导入库：`import calendar`

- 获取某年某月的最大天数

```python
calendar.monthrange(2020, 2)[1]
```

## 第三方库

### dateutil 库

### pendulum 库

#### 官方文档

https://pendulum.eustace.io/

---

> 附：

```plain
术语表：

UTC (Coordinated Universal Time, 国际协调时间)
以原子时秒长为基础，在时刻上尽量接近于世界时的一种时间计量系统。

GMT (Greenwich Mean Time, 格林威治标准时间)
格林尼治所在地的标准时间，也是表示地球自转速率的一种形式。以地球自转为基础的时间计量系统。

DST (Daylight Saving Time, 日光节约时间)
为了节约能源人为规定的一种时间。

CST
可视为如下4个不同的时区的缩写：
美国中部时间：Central Standard Time (USA) UT-6:00
澳大利亚中部时间：Central Standard Time (Australia) UT+9:30
中国标准时间：China Standard Time UT+8:00
古巴标准时间：Cuba Standard Time UT-4:00

时间参数格式：

%a 星期几的简写
%A 星期几的全称
%b 月份的简写
%B 月份的全称
%c 标准的日期的时间串
%C 年份的后两位数字
%d 十进制表示的每月的第几天
%D 月/天/年
%e 在两字符域中，十进制表示的每月的第几天
%F 年-月-日
%g 年份的后两位数字，使用基于周的年
%G 年分，使用基于周的年
%h 简写的月份名
%H 24小时制的小时
%I 12小时制的小时
%j 十进制表示的每年的第几天
%m 十进制表示的月份
%M 十时制表示的分钟数
%n 新行符
%p 本地的AM或PM的等价显示
%r 12小时的时间
%R 显示小时和分钟：hh:mm
%S 十进制的秒数
%t 水平制表符
%T 显示时分秒：hh:mm:ss
%u 每周的第几天，星期一为第一天 （值从0到6，星期一为0）
%U 第年的第几周，把星期日做为第一天（值从0到53）
%V 每年的第几周，使用基于周的年
%w 十进制表示的星期几（值从0到6，星期天为0）
%W 每年的第几周，把星期一做为第一天（值从0到53）
%x 标准的日期串
%X 标准的时间串
%y 不带世纪的十进制年份（值从0到99）
%Y 带世纪部分的十制年份
%z，%Z 时区名称，如果不能得到时区名称则返回空字符。
%% 百分号
```

> 参考来源：  
> https://docs.python.org/zh-cn/3/library/time.html  
> https://docs.python.org/3/library/datetime.html#  
> https://zhuanlan.zhihu.com/p/23679915
