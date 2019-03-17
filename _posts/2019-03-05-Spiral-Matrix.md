---
layout: post
title:  "54. Spiral Matrix"
date: 2019-03-05 21:43:23 -0400
categories: articles
---
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

Example 1:
```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```
Example 2:
```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        if(matrix.size() == 0 || matrix[0].size() == 0 )
            return res;
        int left = 0;
 		int right = matrix[0].size() - 1;
 		int top = 0;
 		int down = matrix.size() - 1;
 		while( left <= right && top <= down ){
 			for (int i = left; top <= down && i <= right; ++i){
 				res.push_back(matrix[top][i]);
 			}
 			top++;

 			for (int i = top; left <= right && i <= down; ++i){
 				res.push_back(matrix[i][right]);
 			}
 			right--;

 			for (int i = right; top <= down && i >= left; --i){
 				res.push_back(matrix[down][i]);
 			}
 			down--;

 			for (int i = down; left <= right && i >= top ; --i){
 				res.push_back(matrix[i][left]);
 			}
 			left++;
 		}
 		return res;
    }
};
```