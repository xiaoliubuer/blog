---
layout: post
title:  "119. Pascal's Triangle II"
date: 2019-04-27 17:26:00 -0400
categories: articles
---

Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle.

Note that the row index starts from 0.


In Pascal's triangle, each number is the sum of the two numbers directly above it.

Example:

Input: 3
Output: [1,3,3,1]
Follow up:

Could you optimize your algorithm to use only O(k) extra space?


```c++
// Accepted!!
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> res;
        if (rowIndex == 0) return vector<int>(1,1);
        
        vector<vector<int>> temp(rowIndex + 1, vector<int>(rowIndex + 1, 0));
        
        for ( int i = 0; i < rowIndex + 1; i++ ){
            temp[i][0] = 1;
            temp[i][i]  = 1;
            if ( i <= 1 ) continue;
            for ( int j = 0; j <= i; j++ ){
                if ( j < 1 ) continue;
                temp[i][j] = temp[i-1][j-1] + temp[i-1][j];
            }
        }
        return temp[rowIndex];
    }
};
```

```c++
//Accepted!
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> res(rowIndex + 1, 0);
        res[0]  = 1;
        for (int i = 0; i < rowIndex + 1; i++ ) {
            for ( int j = i; j > 0; j-- ) {
                res[j] = res[j] + res[j-1];
            }
        }
        return res;
    }
};
```