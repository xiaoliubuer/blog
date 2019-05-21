---
layout: post
title:  "739. Daily Temperatures"
date: 2019-05-09 03:22:00 -0400
categories: articles
---

Given a list of daily temperatures T, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead.

For example, given the list of temperatures T = [73, 74, 75, 71, 69, 72, 76, 73], your output should be [1, 1, 4, 2, 1, 1, 0, 0].

Note: The length of temperatures will be in the range [1, 30000]. Each temperature will be an integer in the range [30, 100].

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        vector<int> res(T.size());
        stack<int> st;
        for ( int i = T.size() - 1; i >= 0; i-- ) {
            while ( !st.empty() && T[i] >= T[st.top()]) st.pop();
            res[i] = st.empty() ? 0 : st.top() - i;
            st.push(i);
        }
        return res;
    }
};
```