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

Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

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
