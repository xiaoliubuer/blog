---
layout: post
title:  "363. Max Sum of Rectangle No Larger Than K"
date: 2018-10-27 20:06:23 -0400
categories: articles
---
Given a non-empty 2D matrix matrix and an integer k, find the max sum of a rectangle in the matrix such that its sum is no larger than k.

Example:

Input: matrix = [[1,0,1],[0,-2,3]], k = 2
Output: 2 
Explanation: Because the sum of rectangle [[0, 1], [-2, 3]] is 2, and 2 is the max number no larger than k (k = 2).
Note:

The rectangle inside the matrix must have an area > 0.
What if the number of rows is much larger than the number of columns?

# 思路:
Input: matrix = [
	[1,    0,    1],
	[0,    -2,   3]
	], 
	k = 2
我操，是一个最大的matrix，那么单个一个也算matrix吗？

这个如何去计算是一个很有意思的事情。

1， 0， 1+0， 1，1+0， 1+0+1

在矩阵里面如何进行遍历，加减，用怎样的顺序来进行加减是个很有意思的问题。
就是把所有的情况找出来，然后比较结果.

那么好了，这怎么来计算和遍历呢？

就像是又个一个点，在矩阵了扫描。然后扫到一个位置上，就能算出

0, 0 和 x，y的sum，这个怎么实现？
```c++
class Solution {
    public:
int maxSumSubmatrix(vector<vector<int>>& matrix, int k) {
    if (matrix.empty()) return 0;
    int row = matrix.size(), col = matrix[0].size(), res = INT_MIN;
    for (int l = 0; l < col; ++l) {
        vector<int> sums(row, 0);
        for (int r = l; r < col; ++r) {
            for (int i = 0; i < row; ++i) {
                sums[i] += matrix[i][r];
            }
            
            // Find the max subarray no more than K 
            set<int> accuSet;
            accuSet.insert(0);
            int curSum = 0, curMax = INT_MIN;
            for (int sum : sums) {
                curSum += sum;
                set<int>::iterator it = accuSet.lower_bound(curSum - k);
                if (it != accuSet.end()) curMax = std::max(curMax, curSum - *it);
                accuSet.insert(curSum);
            }
            res = std::max(res, curMax);
        }
    }
    return res;
}
};
```

草泥马～～ 4个循环～～ 我操～～
