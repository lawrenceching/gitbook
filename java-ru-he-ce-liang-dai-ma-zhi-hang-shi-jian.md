---
description: 本文将会介绍在Java中常用的4种测量代码执行时间的方法。
---

# Java 如何测量代码执行时间

## 测量

### currentTimeMillis\(\)

```java
long start = System.currentTimeMillis();
doSomething();
long end = System.currentTimeMillis();
long used = end - start;
```

`System.currentTimeMillis()` 可能是我们用得最多的与时间相关的方法。该方法返回自UTC1970年1月1日0时0分0秒起的总毫秒数。该时间一般被成为 Unix time, epoch time 或者 POSIX time\([https://en.wikipedia.org/wiki/Unix\_time](https://en.wikipedia.org/wiki/Unix_time)\)。

虽然该方法很简单和直观，但是如 System.currentTimeMillis\(\) 的 Javadoc 所言，它也有一点点缺点。首先，System.currentTimeMillis\(\) 的返回值并不精确。其返回值依赖于底层操作系统。不同的操作系统可能有不同的时间精度（未必是毫秒级）。不同设备间的当前时间会有一定的误差。如果有人手动设置了操作系统的当前时间，System.currentTimeMillis\(\) 也会返回对应的错误结果（因而，从客户端得到的 unix time 时间戳未必是可信的）。

### nanoTime\(\)

顾名思义，nanoTime\(\) 返回纳秒级别的时间戳。但是该时间戳不是指当前时间。其返回值**只能**用于测量程序执行时间。具体细节可以参看 Javadoc。

```text
long startTime = System.nanoTime();
// ... the code being measured ...
long elapsedNanos = System.nanoTime() - startTime
```

如果需要把纳秒转换成毫秒，只需要简单除以 1,000,000。

```text
long elapsedMillis = elapsedNanos / 1000000
```

虽然 nanoTime\(\) 的返回值精确到纳秒，但是它并不保证纳秒级别的准确性。

### Instant/Duration

//TODO:

### StopWatch

//TODO:

## 记录

### 直接输出到日志或文件

### 异步日志

### 批量写入

### 分盘

## 分析

### CSV 和 Excel

### 时序数据库和数据可视化

Influxdb/Prometheus and Grafana

### 日志分析平台

ELK/Splunk

CCSVCSVCScsd

