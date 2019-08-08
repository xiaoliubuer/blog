---
layout: post
title:  "530. Minimum Absolute Difference in BST"
date: 2019-08-07 21:04:00 -0400
categories: articles
---
Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.

Example:
```
Input:

   1
    \
     3
    /
   2

Output:
1
```
Explanation:
```
The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).
```
Note: There are at least two nodes in this BST.

```c++
class Solution {
public:
    TreeNode *prev;
    void util(TreeNode *root, int &res)
    {
        if(!root)
            return;
        util(root->left, res);
        if(prev)
            res = min(res, abs(root->val - prev->val));
        prev = root;
        util(root->right, res);
    }
    int getMinimumDifference(TreeNode* root) {
        prev = nullptr;
        int res = INT_MAX;
        util(root, res);
        return res;
    }
};
```
```c++
class Solution {
    int minDistance = INT_MAX;
    int last = -1;
    public:
    int getMinimumDifference(TreeNode* root) {
        traverse(root);
        return minDistance;
    }
    void traverse(TreeNode* root){
        if (root != nullptr){
            traverse(root->left);
            enqueue(root->val);
            traverse(root->right);
        }
    }
    void enqueue(int val){
        if (last >= 0) minDistance = min(minDistance, val - last);
        last = val;
    }
};
```