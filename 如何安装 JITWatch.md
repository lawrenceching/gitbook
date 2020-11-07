# 如何安装 JITWatch 

JITWatch 是一款用于分析和可视化 HotSpot JIT Compiler 的工具。JITWatch 是基于 JavaFX 开发的。而 2020 年的现在，可能很多人已经不选择 Oracle JDK 作为默认的 JDK 版本，而其他版本的 JDK 可能没有自带 JavaFX 包，安装和使用 JITWatch 会稍显麻烦。

## TL;DR

```bash
# 获取源代码
git clone https://github.com/AdoptOpenJDK/jitwatch.git

# 构建
cd gitwatch
mvn clean install -DskipTests=true

# 启动 JITWatch 如果你使用的是自带了 JavaFX 的 JDK 版本，例如 Oracle JDK。
# ./launchUI.sh

# 下载对应的 OpenJFX 
wget https://chriswhocodes.com/downloads/openjfx-8u60-sdk-overlay-osx-x64.zip

# 设置 CLASSPATH
export CLASSPATH=/path/to/openjfx-8u60-sdk-overlay-osx-x64/jre/lib/ext/jfxrt.jar

# 启动 JITWatch
./launchUI.sh

```

![image-20201107121754082](.gitbook/assets/image-20201107121754082.png)