---
description: 如何在 Bash 中加减称呼
---

# 如何在 Bash 中执行数值计算

| 操作符 | 描述 |
| :--- | :--- |
| id++, id-- | 运算后自增 |
| ++id, --id | 自增后运算 |
| -, + | 符号（1, -1, +1） |
| !, ~ | 逻辑取反、按位取反 |
| \*\* | 求幂 |
| \*, /, % | 乘法、除法、求余 |
| +, - | 加减 |
| &lt;&lt;, &gt;&gt; | 左移、右移 |
| &lt;=, &gt;=, &lt;, &gt; | 算术比较 |
| ==, != |  |
| & | sdfbitwise AND |
| ^ | bitwise XOR |
| \| | bitwise OR |
| && | logical AND |
| \|\| | logical OR |
| expression ? expression : expression | conditional operator |
| =, \*=, /=, %=, +=, -=, &lt;&lt;=, &gt;&gt;=, &=, ^=, \|= | assignment |

```text
let sum=1+2+3+4
echo "Sum: $sum"

i=0
i=$((++i))
echo "i: $i"

let minus=sum-i
echo "minus: $minus"

let division=minus/3
echo "division: $division"

let exponentiation=$((2**division))
echo "exponentiation: $exponentiation"

true=1
false=0
echo "\!$true=$((! true))  \!$false=$((! false))"
echo "$true&&$true=$((true&&true))"
echo "$true&&$false=$((true&&false))"

echo "Result: $((true ? 1000 : 2000))"
```



