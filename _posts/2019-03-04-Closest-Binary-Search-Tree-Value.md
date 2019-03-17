---
layout: post
title:  "270. Closest Binary Search Tree Value"
date: 2019-03-04 21:09:23 -0400
categories: articles
---
Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

Note:

Given target value is a floating point.
You are guaranteed to have only one unique value in the BST that is closest to the target.
Example:
```
Input: root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3

Output: 4
```
```c++
class Solution {
public:
    void getResult(TreeNode *node, double target, double &result){
        if(!node)return;
        if(abs((double)node->val-target)<abs(target-result)) result = (double)node->val;
        if((double)node->val > target)
            getResult( node->left,  target, result );
        else if((double)node->val < target)
            getResult( node->right, target, result );
    }
    int closestValue(TreeNode* root, double target) {
        double result = (double)root->val;
        getResult( root, target, result );
        return (int)result;
    }
};
```