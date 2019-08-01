---
layout: post
title:  "* 354. Russian Doll Envelopes"
date: 2018-12-29 09:26:23 -0400
categories: articles
---
You have a number of envelopes with widths and heights given as a pair of integers (w, h). One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? (put one inside other)

Note:
Rotation is not allowed.

Example:
```
Input: [[5,4],[6,4],[6,7],[2,3]]
Output: 3 
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
```

# 题意
就是有一对pair，[w, h], 只有w和h都大的，才能套在最外面。给一个list的pair，问最多能套几层.

# 思路
不会！！！

```c++
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        int n = envelopes.size();
        if (n <= 1) {
            return n;
        }
        sort(envelopes.begin(), envelopes.end(), [](vector<int>& a, vector<int>& b) {
            return a[0] < b[0] or (a[0] == b[0] and a[1] > b[1]);
        });
        vector<int> result;
        for (int i = 0; i < n; i++) {
            auto it = lower_bound(result.begin(), result.end(), envelopes[i][1]);
            if (it != result.end()) {
                *it = envelopes[i][1];
            }
            else {
                result.push_back(envelopes[i][1]);
            }
        }
        return result.size();
    }
};
```
