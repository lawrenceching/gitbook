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
doSomething();
long elapsedNanos = System.nanoTime() - startTime
```

如果需要把纳秒转换成毫秒，只需要简单除以 1,000,000。

```text
long elapsedMillis = elapsedNanos / 1000000
```

虽然 nanoTime\(\) 的返回值精确到纳秒，但是它并不保证纳秒级别的准确性。

### Instant/Duration

Java 的 Instant 类提供了更好的时区支持和时间计算。使用 Instant 类计算执行用时也和上面的两种方法差不多。

```text
Instant start = Instant.now();
doSomething();      
Instant finish = Instant.now();
long timeElapsed = Duration.between(start, finish).toMillis();
```

### StopWatch

[Apache Commons Lang](https://commons.apache.org/proper/commons-lang/) 是一个非常常用的工具类库。它提供了一个专门用于计算执行时间的工具类—— StopWatch。StopWatch 不仅提供了 start\(\), stop\(\) 和 getTime\(\) 的基础方法，还提供了 split\(\), getSplitTime\(\), suspend\(\) 和 resume\(\) 以完成更复杂的计时操作。

```text
StopWatch stopWatch = new StopWatch();

stopWatch.start();
doSomething();

stopWatch.split();
System.out.println("Used " + stopWatch.getSplitTime() + " ms");

stopWatch.suspend();
doSomething();
stopWatch.resume();

stopWatch.stop();
System.out.println("Used " + stopWatch.getTime() + " ms in total");
```

## 记录

### 直接输出到日志或文件

测出代码执行时间后，自然是要把时间记录下来。最普通也就常用的方法，自然是直接写日志。无论是直接输出到 stdout，还是直接写文件，都能满足大部分的应用场景。如果直接把日志输出到 stdout ，还可以通过重定向（`"java -jar hello.jar > output.log"`）持久化日志。

当日志太频繁，写日志这件事本身会影响程序执行效率。此时就要想办法降低写日志对性能的影响。

### 异步日志

就日志而言，最常用的当然是利用 logback 的 AsyncAppender。

如果写日志太频繁，logger.info\(\) 本身就会卡在 IO 上。顺利成章，不能同步，那就异步。

与此同时，也必须注意到异步日志方案本身的一点小问题：

1、不同日志间的先后顺序可能会打乱。  
2、如果 CPU 已经用满，日志可能会来不及打印。  
3、如果磁盘IO已经用满，日志可能会因为来不及打印而堆积在内存里，有潜在的 OOM 风险。

### 批量写入

正如你在日常拷贝文件的时候发现，大文件的拷贝速度比大量零碎文件的拷贝要快好。所以频发而小量地写日志，效率并不高。这种情况在把日志通过 HTTP 或 TCP 协议上传到远端服务器，就更为严重。

如果是写日志，为了提高效率，我们通常把多个时间合并到一起输出。例如每100个请求输出一行：

```text
12,11,8,12,17,5,2,34,26,11,123,25,21,21,.....
```

甚至，你可以直接输出二进制格式的日志文件以追求最高效率。

而远程日志上报方案，一样采取相同的思路。通过在客户端或者日志网关中收集日志，然后批量上传到日志服务器中。

### 分盘

如果一块盘速度不够，那同时写入两块盘，岂不是速度翻倍！（我只在测试环境见过这种骚操作，不确定是否有用在生产环境的案例）。

## 分析

### CSV 和 Excel

虽然 Excel 看着不是程序员的工具，但是其实 Excel 对中小规模的数据处理和可视化非常好用。Excel 支持 CSV 格式文件，把数据输出成 CSV 格式又非常简单，加之大部分公司，哪怕没有 IntelliJ IDEA，也不可能没有微软三件套。可谓是方便至极。

### 时序数据库和数据可视化

数据监控当然是必不可少的需求。通过监控系统的实时性能，运维同学能提早发现问题，提早处理，以此规避可能的业务随时。

要实现这一需求，我们需要用到时序数据库和数据可视化工具。时序数据库负责储存时间序列的数据，可视化工具负责把枯燥的数据以图表的形式展示出来。  
市面上有多款时序数据库, InfluxDB、Graphite 或者 Prometheus 都是经过大量公司验证的时序数据库产品。不过话又说回来，普通的关系型数据库和 NoSQL  类型的数据库也一样可以储存时序数据，例如 ElasticSearch 、Cassandra 或者 MySQL。

在数据可视化方面，Grafana 是当之无愧的首选方案。它不仅支持众多数据库产品，而且开源、免费。

### 日志分析平台

业内常用的另外一种监控方案，则是基于日志的统计分析工具。常用的有 ELK 或 Splunk。这类工具通过从日志提取数据，经过分析、统计后，生成图表。  
在基于时序数据库的监控方案中，程序员需要预先在程序中打点，并且需要把数据上报到舒徐数据库。  
而基于日志分析的监控方案对程序的侵入性更小。如果日志已经事先存在，日志分析工具还能针对过往数据做统计。

不过，有优势也就有劣势。基于日志的统计方式性能非常糟糕。如果要统计时间跨度较长的数据，需要遍历过去所有的日志，按格式提取数据。如果数据量非常大，简单的数据需要耗费几个小时才能出结果。这对监控来说，性价比确实不可接受。

