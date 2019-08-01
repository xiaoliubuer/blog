---
layout: post
title:  "386. Lexicographical Numbers"
date: 2019-07-31 21:44:00 -0400
categories: articles
---

Given an integer n, return 1 - n in lexicographical order.

For example, given 13, return: [1,10,11,12,13,2,3,4,5,6,7,8,9].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

```c++
class Solution {
public:
    vector<int> lexicalOrder(int n) {
        vector<int> res;
        dfs(1, n, res);
        return res;
    }
private:
    void dfs(int i, int n, vector<int>& res) {
        if (i > n) return;
        res.push_back(i);
        dfs(i * 10, n, res);
        if (i % 10 < 9) {
            dfs(i + 1, n, res);
        }
    }
};
