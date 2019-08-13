---
title:  "56 - merge intervals"
tags: 
    - Medium
    - Array
    - Sort
---

Given a collection of intervals, merge all overlapping intervals.

Example 1:
```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

Example 2:
```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## Analysis

- Input `vector<vector<int>>& intervals`: 二维向量
- Output `vector<vector<int>>`: 
- 先对所有区间按照其起始值，从小到大排序
- 然后线性比较除首区间（也就是起始值最低的区间）以外的所有区间：
	- 前尾值小于后首值时，两区间无重合，后区间直接入队尾
	- 前尾值大于或等于后首时，两区间发生首尾重合，因首尾重合，需比较谁的尾值更大，以更新前尾值
- 使用 `std::move()` 强制移动左值 `intervals[i]` 为右值引用，避免 `push_back()`深拷贝，来提升性能
- TC = $$O(n\log(n))$$ 来自 `std::sort()`

## Solution

### 93%

```cpp
// #include <algorithm>

class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        typedef vector<int> Interval;
        
        if (intervals.size() <= 1) return intervals;
        
        vector<Interval> ret;
        sort(intervals.begin(), intervals.end(), [](Interval &a, Interval &b) {
            return a[0] < b[0];
        });
        ret.push_back( std::move(intervals[0]) );
        for (int i = 1; i < intervals.size(); i++) {
            if (ret.back()[1] < intervals[i][0])
                ret.push_back( std::move(intervals[i]) );
            else
                ret.back()[1] = max(ret.back()[1], intervals[i][1]);
        }

        return ret;      
    }
};
```

```
Success
Details 
Runtime: 16 ms, faster than 93.82% of C++ online submissions for Merge Intervals.
Memory Usage: 12.2 MB, less than 100.00% of C++ online submissions for Merge Intervals.
```
