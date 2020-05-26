---
title:  "70 - climbing stairs"
tags: 
    - Easy
    - Dynamic Programming
---
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:
```
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

Example 2:
```
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## Solution

### Time Limit Exceeded, 0%

```c++
/**
 * 普通递归时间复杂度, 非多项式时间, 而是 2^n. 随着输入规模n的增长, 运行时间指数增长, 递归导致运行超时
 */
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1) {
            return 1;
        } else if (n == 2) {
            return 2;
        } else {
            return climbStairs(n-1) +  climbStairs(n-2);    
        }
    }
};
```

```
Submission Detail

31 / 45 test cases passed.
Status: Time Limit Exceeded
Submitted: 31 minutes ago
Last executed input:
45
```

### 100.00%

```c++
/**
 * 采用 带备忘的自顶向下法, 在普通递归程序中, 
 * 加入空间哈希缓存每个子问题的解, 避免子问题重叠情况下重复求解,
 * 从而削减 2^n 指数时间复杂度 (即, time-memory-trade-off) 到多项式时间
 */
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1) {
            return 1;
        } else if (n == 2) {
            return 2;
        } else {
            auto el = mem.find(n);
            if (el == mem.end()) {
                int temp = climbStairs(n-1) +  climbStairs(n-2);
                mem.emplace(std::make_pair(n, temp));
                return temp;
            } else {
                return el->second;
            }
        }
    }
    
    std::map<int, int> mem;
};
```

```
Success
Details 
Runtime: 0 ms, faster than 100.00% of C++ online submissions for Climbing Stairs.
Memory Usage: 6 MB, less than 100.00% of C++ online submissions for Climbing Stairs.
Next challenges:
Min Cost Climbing Stairs
Fibonacci Number
N-th Tribonacci Number
```

### 100.00%

```c++
/**
 * 自底向上法, 由于没有递归函数调用, 不会浪费运行时栈空间
 * 仍然是带记忆的
 */
class Solution {
public:
    int climbStairs(int n) {
        steps.push_back(0); // 输入规模为空时
        steps.push_back(1); // n = 1时
        steps.push_back(2); // n = 2时
        
        // 从 3 开始迭代, 自底向下, 不断迭代, 达到输入规模截止
        for (int i = 3; i <= n; ++i) {
            steps.emplace_back(steps[i-1] + steps[i-2]);
        }
        
        return steps[n];
    }
    
    std::vector<int> steps;
};
```

```
Success
Details 
Runtime: 0 ms, faster than 100.00% of C++ online submissions for Climbing Stairs.
Memory Usage: 6.1 MB, less than 100.00% of C++ online submissions for Climbing Stairs.
Next challenges:
Min Cost Climbing Stairs
Fibonacci Number
N-th Tribonacci Number
```