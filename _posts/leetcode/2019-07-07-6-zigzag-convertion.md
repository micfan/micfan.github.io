---
title:  "6 - zigzag convertion"
tags: 
    - leetcode
    - Medium
    - String
categories: leetcode
---

## Solution

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows == 1) { return s; }
        
        vector<string> v(numRows);
        
        int curr_row = 0;
        int direct = 0;
        for (int i = 0; i < s.length(); i++) {
            if (curr_row == 0) {
                direct = 1;
            }
            if (curr_row == numRows - 1) {
                direct = -1;
            }
            
            v[curr_row] += s[i];
            
            
            curr_row += direct;
        }
        
        string ans;
        for (const auto s: v) {
            ans += s;
        }
        
        return ans;
    }
};
```
