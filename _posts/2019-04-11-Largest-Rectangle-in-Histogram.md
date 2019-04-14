---
layout: post
title:  "84. Largest Rectangle in Histogram"
date: 2019-04-11 08:43:00 -0400
categories: articles
---
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].

The largest rectangle is shown in the shaded area, which has area = 10 unit.

Example:
```
Input: [2,1,5,6,2,3]
Output: 10
```

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& A) {
        int n = A.size(), ans = 0, pos = 0;
        stack<int> st;
        for (int i = 0; i <= n; i++) {
            while (!st.empty() && (i == n || A[st.top()] >= A[i])) {
                pos = st.top(); 
                st.pop();
                ans = max(ans, A[pos] * (st.empty() ? i : i-st.top()-1));
            }
            st.push(i);
        }
        return ans;
    }
};
```