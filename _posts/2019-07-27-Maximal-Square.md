---
layout: post
title:  "221. Maximal Square"
date: 2019-07-27 19:15:00 -0400
categories: articles
---

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

Example:
```
Input: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output: 4
```

```c++

class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if (matrix.empty())
            return 0;
        vector<int> dp(matrix[0].size() + 1, 0);
        int result = 0;
        for (const auto& line : matrix) {
            int s = 0;
            for (int i = 0; i < line.size(); ++i) {
                swap(s, dp[i + 1]);
                dp[i + 1] = line[i] == '1' ? min({dp[i], dp[i + 1], s}) + 1 : 0;
                result = max(result, dp[i + 1]);
            }
        }
        return result * result;
    }
};
```