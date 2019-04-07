---
layout: post
title:  "671. Second Minimum Node In a Binary Tree"
date: 2019-04-05 18:21:01 -0400
categories: articles
---
Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly two or zero sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.

Given such a binary tree, you need to output the second minimum value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

Example 1:
```
Input: 
    2
   / \
  2   5
     / \
    5   7

Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
```
Example 2:
```
Input: 
    2
   / \
  2   2

Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
```
```c++
class Solution {
public:
    int helper(TreeNode* root, int parent){
        if (!root) return parent;
        if (root->val > parent) return root->val;
        int left = helper(root->left, parent);
        int right= helper(root->right,parent);
        if ( left > parent && right > parent) return min(left, right);
        else return max(parent, max(left, right));
    }
    int findSecondMinimumValue(TreeNode* root) {
        if (!root) return -1;
        int left = helper(root->left, root->val);
        int right= helper(root->right,root->val);
        if ( left > root->val && right > root->val) return min(left, right);
        int res = max(root->val, max(left, right));
        return res == root->val ? -1 : res;
    }
};
```