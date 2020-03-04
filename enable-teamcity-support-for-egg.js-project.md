---
description: >-
  Let TeamCity know how to detect code coverage and test cases for egg.js
  project
---

# Enable TeamCity support for egg.js project

![](.gitbook/assets/image%20%281%29.png)

### **All in All**

To make it quick and simple, please update "cov" command in `package.json`

```text
"cov": "egg-bin cov --nyc=\"-r teamcity -r text\" --reporter mocha-teamcity-reporter",
```

And then run `npm run cov` or `npm run ci` .

Note: `npm run test` still prints clean and neat output. Generally you should run `npm run ci` in your CI tool. Keep the ouptut clean and neat for `npm run test` is good for running test cases in your local machine.

### Why and How

TeamCity is a advanced CI tool that can display and analyze the coverage and tests for you. You can check out the details of test coverage and test cases for each build. Futher you can see the trend of them if you concern the codebase quality.

 

![](.gitbook/assets/image%20%288%29.png)

To do that you have to tell TeamCity how to detect those information from build log. There are reporters call teamcity reporters that print cov. and test cases information in a certain partern that TeamCity knows. It looks like:

```text
##teamcity[blockOpened name='Code Coverage Summary']
##teamcity[buildStatisticValue key='CodeCoverageAbsBCovered' value='184']
##teamcity[buildStatisticValue key='CodeCoverageAbsBTotal' value='384']
##teamcity[buildStatisticValue key='CodeCoverageAbsRCovered' value='10']
##teamcity[buildStatisticValue key='CodeCoverageAbsRTotal' value='56']
##teamcity[buildStatisticValue key='CodeCoverageAbsMCovered' value='25']
##teamcity[buildStatisticValue key='CodeCoverageAbsMTotal' value='70']
##teamcity[buildStatisticValue key='CodeCoverageAbsLCovered' value='183']
##teamcity[buildStatisticValue key='CodeCoverageAbsLTotal' value='382']
```

To look at the command again`"cov": "egg-bin cov --nyc=\"-r teamcity -r text\" --reporter mocha-teamcity-reporter"`

`--nyc=\"-r teamcity -r text\"` is used to tell nyc that to use teamcity reporter to print code coverage.

`--reporter mocha-teamcity-reporter"` is the parameter for test  reporter, given that the undelrying test framework in egg.js is Mocha.

