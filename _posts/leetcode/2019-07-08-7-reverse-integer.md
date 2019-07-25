---
title:  "7 - reverse integer"
tags: 
    - leetcode
    - Easy
    - Math
categories: leetcode
---
Given a 32-bit signed integer, reverse digits of an integer.


Example 1:
```
Input: 123
Output: 321
```

Example 2:
```
Input: -123
Output: -321
```

Example 3:
```
Input: 120
Output: 21
```

Note:

Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [$$−2^{31}$$,  $$2^{31} − 1$$]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## Analysis

- long 类型可以防止计算过程中的数值溢出，CPP隐式转换 long 为 int。
- 设 x 包含 n 个数字， 自左到右依次为 1th, 2th, 3th, ... nth; 
    - `x % 10`，可得最低位个位的数值，即`x[nth]`
    - `x / 10`，可得除最低位个位的数值之外的数值，即`x[:-1]`。（注，这里使用Python字符串切片语法表达）
- 循环不变性质：
    - 初始化: 循环的第一次迭代之前，它为真。

        `result = result * 10 + x % 10;`, 得到原数值的个位`x[nth]`

    - 保持： 如果循环的某次迭代之前它为真，那么下次迭代之前它仍然为真。

        `result = result * 10 + x % 10;`, 循环的第1次看，个位`x[nth]`乘以10升位，新的个位`x[nth]`补充进来

    - 终止：在循环终止时，不变式为我们提供了一个有用的性质，该性质有助于证明算法是正确的。
        
        终止条件为 for 循环内的 `(bool)x == true`，这个 x 保存了 `x[:-1]` ，当 `x == 0` 时，输入被消耗完毕

## Solution

```cpp
// #include <cmatch>

class Solution {
public:
    int reverse(int x) {
        signed long long int result = 0;
        for (; x; x/=10)
        {
            result = result * 10 + x % 10;
        }
        return (abs(result) > (pow(2, 31)-1)) ? 0 : result;
    }
};
```

```
Success
Details 
Runtime: 0 ms, faster than 100.00% of C++ online submissions for Reverse Integer.
Memory Usage: 8.3 MB, less than 36.56% of C++ online submissions for Reverse Integer.
```
