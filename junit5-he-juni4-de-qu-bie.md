# JUnit5 和 JUni4 的区别

### 标注

| 功能 | JUNIT 4 | JUNIT 5 |
| :--- | :--- | :--- |
| 生命一个测试 | `@Test` | `@Test` |
| 在所有测试运行之前执行指定方法 | `@BeforeClass` | `@BeforeAll` |
| 在所有测试运行之后执行指定方法 | `@AfterClass` | `@AfterAll` |
| 在每个测试运行前执行指定方法 | `@Before` | `@BeforeEach` |
| 在每个测试运行后执行指定方法 | `@After` | `@AfterEach` |
| 在方法或类级别禁止测试 | `@Ignore` | `@Disabled` |
| 动态测试 | 不支持 | `@TestFactory` |
| 嵌套测试 | 不支持 | `@Nested` |
| 标签和分类 | `@Category` | `@Tag` |
| 扩展 | 不支持 | `@ExtendWith` |

### 断言

在断言方面，JUnit5 增加了两个新的方法 —— assertThrows\(\) 和 assertAll\(\)。

```java
@Test
void exceptionTesting() {
    Exception exception = assertThrows(ArithmeticException.class, () ->
        calculator.divide(1, 0));
    assertEquals("/ by zero", exception.getMessage());
}

@Test
void groupedAssertions() {
    // In a grouped assertion all assertions are executed, and all
    // failures will be reported together.
    assertAll("person",
        () -> assertEquals("Jane", person.getFirstName()),
        () -> assertEquals("Doe", person.getLastName())
    );
}
```

与此同时，JUnit5 原生支持了 Kotlin。例如，上述的 assertAll\(\) 在 Kotlin 下也可以这样写。

```kotlin
@Test
fun `grouped assertions from a stream`() {
    assertAll("People with first name starting with J",
        people
            .stream()
            .map {
                // This mapping returns Stream<() -> Unit>
                { assertTrue(it.firstName.startsWith("J")) }
            }
    )
}
```

### 其他

其他方面，JUnit5 和 JUnit4 大致相同。Assumption、Tag、Test Suite 等功能基本一致。

### 参考文献

{% embed url="https://howtodoinjava.com/junit5/junit-5-vs-junit-4/" %}

{% embed url="https://junit.org/junit5/docs/current/user-guide/" %}



