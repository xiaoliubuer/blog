---
layout: post
title:  "216. Combination Sum III"
date: 2019-07-27 18:55:00 -0400
categories: articles
---

Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

Note:
```
All numbers will be positive integers.
The solution set must not contain duplicate combinations.
```
Example 1:
```
Input: k = 3, n = 7
Output: [[1,2,4]]
```
Example 2:
```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

```c++
class Solution {
public:
  void combination(vector<vector<int>>& result, vector<int> sol, int k, int n) {
    if (sol.size() == k && n == 0) { result.push_back(sol); return ; }
    if (sol.size() < k) {
      for (int i = sol.empty() ? 1 : sol.back() + 1; i <= 9; ++i) {
        if (n - i < 0) break;
        sol.push_back(i);
        combination(result, sol, k, n - i);
        sol.pop_back();
      }
    }
  }

  vector<vector<int>> combinationSum3(int k, int n) {
    vector<vector<int>> result;
    vector<int> sol;
    combination(result, sol, k, n);
    return result;
  }
};
```