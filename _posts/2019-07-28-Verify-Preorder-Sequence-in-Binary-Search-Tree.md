---
layout: post
title:  "255. Verify Preorder Sequence in Binary Search Tree"
date: 2019-07-28 16:09:00 -0400
categories: articles
---
Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.
You may assume each number in the sequence is unique.
Consider the following binary search tree: 
```
     5
    / \
   2   6
  / \
 1   3
```
Example 1:
```
Input: [5,2,6,1,3]
Output: false
```
Example 2:
```
Input: [5,2,1,3,6]
Output: true
```
Follow up:
Could you do it using only constant space complexity?


```c++
class Solution {
public:
    bool verifyPreorder(vector<int>& preorder) {
        stack<int> stk;
        int lower_bound = INT_MIN;
        for(int i = 0; i < preorder.size(); i++){
            if(stk.empty() || preorder[i] < preorder[i - 1]){
                if(preorder[i] <= lower_bound) return false;
                stk.push(preorder[i]);
            }else{
                while(!stk.empty() && stk.top() < preorder[i]){
                    lower_bound = stk.top();
                    stk.pop();
                }
                stk.push(preorder[i]);
            }
        }
        
        return true;
    }
};
```