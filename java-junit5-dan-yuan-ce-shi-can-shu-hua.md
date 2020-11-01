---
description: 本人介绍了在 JUnit5 框架下，如何优雅地只写一份单元测试，测试不同的测试用例——参数化测试。
---

# Java JUnit5 单元测试参数化

## TL;DR

JUnit5 可以通过 `ParameterizedTest` 和 SourceArguments 两个 annotation 来参数化一个单元测试。

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

public class Junit5Test {

  @ParameterizedTest
  @ValueSource(ints = {1, 0, -1, Integer.MIN_VALUE, Integer.MAX_VALUE})
  void parameterizedTest(int num) {
    assertEquals("" + num, String.valueOf(num));
  }

}
```

而上方代码中的 `@ValueSource` 就是 Source Argument，用于标注单元测试的参数。每个参数通过测试方法的参数传入。  


[@ValueSource](https://github.com/junit-team/junit5/blob/main/junit-jupiter-params/src/main/java/org/junit/jupiter/params/provider/ValueSource.java) 除了支持 ints 外，还支持：

```text
short 
byte
int
long
float
double
char
boolean
java.lang.String
java.lang.Class
```

除了 `@ValueSource` 之外，JUnit5 还提供了其他的 SourceArguments。例如：  
@NullSource  
@EmptySource  
@MethodSource  
@CsvSource  
@ArgumentsSource  
详细用法可以参考后文或者官方文档：[https://junit.org/junit5/docs/current/user-guide/\#writing-tests-parameterized-tests-sources](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests-sources)

## 什么是参数化

参数化的意识是，使得一个单元测试可以根据外部参数执行不同的测试。在编码单元测试时，一个频繁遇到的场景是，一段代码你需要测试各种不同的场景。

  
例如，如果你的代测代码接受数字作为输入，那么通常你需要测试，0、正数、负数、最大值、最小值。

  
或者，如果你的代测代码接受 List 作为输入，那么你就需要测试 null, 空 List、只有一个 Item 和有多个 Item 的情况。

  
如果单元测试无法参数化，你可能不得不复制粘贴好几次，把同一段单元测试代码复制几次，再修改相应的输入数据。又或者，你可以选择写个循环，遍历你的测试数据。

然而这样做都不够方便。第一种方法重复劳动太多，不符合 DRY 的代码原则。第二种方法，若中间的某个测试数据测出了问题，不能直观地看到是哪一组数据测出了问题。

而 JUnit5 的单元测试参数化，就是解决这样的困扰。在上方的 TL;DR 中，我通过 @ParameterizedTest 和 @ValueSource 定义了一个单元测试的输入数据。如果你在 IDE 里运行，你可以看到没一组数据都单独列出来，非常清晰明了。

![](.gitbook/assets/image%20%2830%29.png)

### 参数化一组测试数据

### 参数化 Null 和 Empty 值

### 参数化枚举数据

### 参数化 CSV 格式的数据

### 自定义数据源

### 参数转换



### 参考文献

{% embed url="https://junit.org/junit5/docs/current/user-guide/\#writing-tests-parameterized-tests" %}

[https://github.com/junit-team/junit5](https://github.com/junit-team/junit5)







