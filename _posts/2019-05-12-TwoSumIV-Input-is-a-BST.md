---
layout: post
title:  "653. Two Sum IV - Input is a BST"
date: 2019-05-12 16:39:00 -0400
categories: articles
---

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

Example 1:
```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
``` 

Example 2:
```
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

Output: False
```
```c++
class Solution {
public:
    bool helper(TreeNode* root, int k, unordered_set<int>& cache){
        if (!root) return false;
        if ( cache.find( k - root->val) != cache.end()) return true;
        cache.insert(root->val);
        return helper(root->left, k, cache) || helper(root->right, k, cache);
    }
    bool findTarget(TreeNode* root, int k) {
        unordered_set<int> cache;
        return helper(root, k, cache);
    }
};
```