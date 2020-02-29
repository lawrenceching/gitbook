---
description: egg.js 如何运行指定文件的单元测试或者指定的一个单元测试
---

# egg.js 如何运行指定的单元测试

### 运行指定文件的单元测试

通过 TESTS 变量指定单元测试文件

```text
TESTS=test/x.test.js npm test
```

通过命令行参数指定单元测试文件

```text
npm test "test/**/test.js"
```

### 运行指定的单元测试

egg.js 使用 Mocha 作为测试框架。在 Mocha 中，你可以把 `describe()` 或者 `it()` 函数替换成`describe.only()` 或者 `it.only()`

在下面的例子中，运行`npm test`，只有 “throw error if user id is not valid” 这个测试会被运行。

```text
describe('test/app/controller/user.test.js', () => {
    
    it('should return full list of user', () => {
    }
    
    // Use `it.only()` instead of `it()` to run below test only
    it.only('throw error if user id is not valid', () => {
    }    
)
```

### 参考文献

{% embed url="https://github.com/eggjs/egg/blob/master/docs/source/en/core/development.md\#run-specific-test-file" %}

{% embed url="https://mochajs.org/\#exclusive-tests" %}



