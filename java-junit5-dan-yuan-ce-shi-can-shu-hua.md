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

你可以通过 @ValueSource 定义一组简单的测试数据。 @ValueSource 支持 8 中基本类型、String 字符串和 Class 类型。

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

下方代码演示了如何定义一组 Class 类型的测试参数。

```java
@ParameterizedTest
@ValueSource(classes = {String.class, TimeUnit.class, RuntimeException.class})
void parameterizedTestWithClasses(Class clazz) {
    System.out.println(clazz.getCanonicalName());
}
```

### 参数化 Null 和 Empty 值

JUnit5 提供了关于 null 值和空值的数据源——@NullSource、@EmptySource 和 @NullAndEmptySource。 其中空值是指： 空字符串： "" 空数组： String\[\]、int\[\] 甚至二维数组 char\[\]\[\] 空容器: 空的 List, Set 或者 Map @NullAndEmptySource 为 @NullSource 和 @EmtpySource 的组合 Annotation。

当然，通过 Annotation 去定义一个空的测试参数似乎有点鸡肋。这三个数据源的主要用途是与 @ValueSource 一起使用。由于 Java 语法的本身限制，@ValueSource 是无法接受 null 值的，而 null 和 empty 又是单元测试中必不可少的 boundry case。为了解决这个问题，@NullSource、@EmptySource 和 @NullAndEmptySource 应运而生。

```java
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = {"A", "B", "C"})
  void parameterizedTestForString(String str) {
  System.out.println(str);
}
```

### 参数化枚举数据

枚举，也是单元测试的常客。你的 if 和 switch 肯定经常与 Enum 配合使用，对吧？

```java
@ParameterizedTest
@EnumSource
void parameterizedTestForEnum(TimeUnit unit) {
    System.out.println(unit.name());
}
```

JUnit5 提供了 @EnumSource。@EnumSource 会根据方法的输入参数判断你要测试的枚举类型。然后注入所有的枚举值。上方的例子中，JUnit5 会生成 TimeUnit 的 NANOSECONDS、MICROSECONDS、MILLISECONDS等一共7个测试用例。

如果你不想测试所有枚举值，@EnumSource 接受 names 参数，用于指定需要测试的枚举值。

```java
@ParameterizedTest
@EnumSource(names = { "DAYS", "HOURS" })
void parameterizedTestForEnum(TimeUnit unit) {
  System.out.println(unit.name());
}
```

而有时候，枚举的值太多，相比指定你想测试什么，你可能更想指定你不想测试什么。@EnumSource 提供了 mode 参数。该参数指定你想如何匹配枚举值。mode 支持 INCLUDE、EXCLUDE、MATCH\_ALL 或 MATCH\_ANY。嗯，顾名思义，不多做解释。

### 参数化 CSV 格式的数据

上述的众多 Argument Source，有一个比较尴尬的缺陷。它们只能指定一维的测试数据。如果你想指定多维的数据，或者你想指定输入数据和测试结果，事情就比较尴尬了。为了解决这个问题，你可以使用 @CsvSource 。@CsvSource 接受多行 csv 格式的数据，并根据 csv 生成单元测试。

```java
@ParameterizedTest
@CsvSource({
    "Apple     , 5 ",
    "Banana    , 6 ",
    "Watermelon, 10"
})
void parameterizedTestForCsv(String word, int len) {
  assertEquals(len, word.length());
}
```

代码相当直观，不用多做解释。但有几点需要注意

#### 如果数据带有逗号，你需要用单引号括起来：

@CsvSource\({ "apple, **'lemon, lime'**" }\)

#### 空字符串

@CsvSource\({ "apple, **''**" }\)

#### null 值

@CsvSource\({ "apple, " }\) 或者指定 nullValues 参数： @CsvSource\(value = { "apple, banana, NIL" }, nullValues = "NIL"\)

### 自定义数据源

### 参数转换

### 参考文献

{% embed url="https://junit.org/junit5/docs/current/user-guide/\#writing-tests-parameterized-tests" caption="" %}

[https://github.com/junit-team/junit5](https://github.com/junit-team/junit5)

q

