# How to do math in Bash

| Arithmetic Operator | Description |
| :--- | :--- |
| id++, id-- | variable post-increment, post-decrement |
| ++id, --id | variable pre-increment, pre-decrement |
| -, + | unary minus, plus |
| !, ~ | logical and bitwise negation |
| \*\* | exponentiation |
| \*, /, % | multiplication, division, remainder \(modulo\) |
| +, - | addition, subtraction |
| &lt;&lt;, &gt;&gt; | left and right bitwise shifts |
| &lt;=, &gt;=, &lt;, &gt; | comparison |
| ==, != | equality, inequality |
| & | bitwise AND |
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

