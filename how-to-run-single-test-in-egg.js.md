---
description: The ways to run single test or test in single file in egg.js
---

# How to run single test in egg.js

It's quite important to know how to run specific test if you have tons of unit tests in your project. Below are 3 ways to you can use to achieve your goal.

### Option 1: TESTS=test/x.test.js npm test

By setting variable `TESTS`, you can run unit tests that only in a file you want.

### Option 2: npm test "test/\*\*/x.test.js"

Option 2 equivalents to option 1. You can put the file path as command line argument to tell egg.js runs only unit tests in `test/**/x.test.js`

### Option 3: describe.only\(\) and it.only\(\)

Somes even in a single .test.js file, you may have more than 10 test cases for one controller. Given that the underlying test frameework is Mocha, you can definitely use `describe.only()` or `it.only()` to run single test or single block of tests.

For example, in below snippet, if you run `npm test` or `yarn test`, only "throw error if user id is not valid" will be run.

```javascript
describe('test/app/controller/user.test.js', () => {

    it('should return full list of user', () => {
    }

    // Use `it.only()` instead of `it()` to run below test only
    it.only('throw error if user id is not valid', () => {
    }    
)
```

## References

[https://mochajs.org/\#exclusive-tests](https://mochajs.org/#exclusive-tests)  
[https://github.com/eggjs/egg/blob/master/docs/source/en/core/development.md\#run-specific-test-file](https://github.com/eggjs/egg/blob/master/docs/source/en/core/development.md#run-specific-test-file)

