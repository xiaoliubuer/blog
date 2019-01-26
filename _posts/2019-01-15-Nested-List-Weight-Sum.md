---
layout: post
title:  "339. Nested List Weight Sum"
date: 2019-01-15 20:34:23 -0400
categories: articles
---
Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Example 1:
```
Input: [[1,1],2,[1,1]]
Output: 10 
Explanation: Four 1's at depth 2, one 2 at depth 1.
```
Example 2:
```
Input: [1,[4,[6]]]
Output: 27 
Explanation: One 1 at depth 1, one 4 at depth 2, and one 6 at depth 3; 1 + 4*2 + 6*3 = 27.
```
# Function signature
```c++
class Solution {
public:
    int depthSum(vector<NestedInteger>& nestedList) {
        
    }
};
```
# 题意
就是给一个NestedInteger的vector，返回他们的和。层数 * value 的值的sum。
# 想法

# 尝试解解
```c++
// Accepted !! 有意思！
class Solution {
public:
    void helper(vector<NestedInteger> list, int level, int& res){
        for (int i = 0; i < list.size(); i++) {
            if (list[i].isInteger()) res += list[i].getInteger() * level;
            else helper(list[i].getList(), level + 1, res);
        }
    }
    int depthSum(vector<NestedInteger>& nestedList) {
        int res = 0;
        if (nestedList.size() == 0) return res;
        helper(nestedList, 1, res);
        return res;
    }
};
```