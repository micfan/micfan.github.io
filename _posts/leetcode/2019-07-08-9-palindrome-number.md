---
layout: article
title:  "9. palindrome number"
tags: 
    - leetcode
    - Easy
    - Math
categories: leetcode
---


{% highlight c++ %}

class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) { return false; }
        
        int num = x;
        long reverse = 0;
        while (num > 0) {
            // int dig = num % 10;
            reverse = reverse * 10 + num % 10 /* dig: to speed up,
                                                 skip the variable assignment statement */;
            num /= 10;
        }
        
        return reverse == x;
    }
};

{% endhighlight %}
