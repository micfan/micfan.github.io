---
layout: article
title:  "7. reverse integer"
tags: 
    - leetcode
    - Easy
    - Math
categories: leetcode
---


{% highlight c++ %}

// #include <cmatch>

class Solution {
public:
    int reverse(int x) {
        signed long long int result = 0;
        for (; x; x/=10)
        {
            result = result * 10 + x % 10;
        }
        if (abs(result) > (pow(2, 31)-1)) {
            return 0;
        } else {
            return result;
        }
    }
};

{% endhighlight %}
