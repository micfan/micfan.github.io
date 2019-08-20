---
title:  "57 - insert interval"
tags: 
    - Hard
    - Array
    - Sort
---

Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

Example 1:
```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

Example 2:
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## Analysis

- Input `vector<vector<int>>& intervals`: 二维向量
- Output `vector<vector<int>>`: 
- TC = $$O(n\log(n))$$ 来自 `std::equal_range`

## Solution

### 83%

```cpp
class Solution {
public:
    
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        typedef vector<int> Interval;
        
        auto compare = [] (const Interval &intv1, const Interval &intv2) {
            return intv1[1] < intv2[0]; 
        };
        auto range = std::equal_range(intervals.begin(), intervals.end(), newInterval, compare);
        auto itr1 = range.first, itr2 = range.second;
        if (itr1 == itr2) {
            intervals.insert(itr1, newInterval);
        } else {
            itr2--;
            (*itr2)[0] = min(newInterval[0], (*itr1)[0]);
            (*itr2)[1] = max(newInterval[1], (*itr2)[1]);
            intervals.erase(itr1, itr2);
        }
        return intervals;
    }
};
```

```
Success
Details 
Runtime: 16 ms, faster than 83.65% of C++ online submissions for Insert Interval.
Memory Usage: 12.2 MB, less than 100.00% of C++ online submissions for Insert Interval.
```
