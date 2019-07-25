---
title:  "2 - add two numbers"
tags: 
    - leetcode
    - Medium
    - Linked List
    - Math
categories: leetcode
---

## Solution

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* ret = new ListNode(0);
        ListNode* curr = ret;
        ListNode* tmp;
        
        while (!(
            l1 == NULL && l2 == NULL
        )) {
            tmp = addTwo(l1, l2);
            curr->next = tmp;
            curr = tmp;
            if (l1 != NULL) {
                l1 = l1->next;
            }
            if (l2 != NULL) {
                l2 = l2->next;
            }
        }
        
        tmp = ret->next;
        while(tmp != NULL) {
            if (tmp->val >= 10) {
                tmp->val %= 10;
                if (tmp->next == NULL) {
                    tmp->next = new ListNode(1);
                } else {
                    tmp->next->val += 1;
                }
            }
            tmp = tmp->next;
        }
        
        return ret->next;
    }
    
    ListNode* addTwo(ListNode* p1, ListNode* p2) {
        // 单节点
        int val = 0;
        if (p1 != NULL) {
            val += p1->val;
        }
        if (p2 != NULL) {
            val += p2->val;
        }
        ListNode *ret = new ListNode(val);
        return ret;
    }
};
```
