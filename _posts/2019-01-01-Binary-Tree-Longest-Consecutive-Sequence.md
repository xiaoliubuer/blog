---
layout: post
title:  "298. Binary Tree Longest Consecutive Sequence"
date: 2019-01-01 14:18:23 -0400
categories: articles
---
Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

Example 1:
```
Input:

   1
    \
     3
    / \
   2   4
        \
         5

Output: 3

Explanation: Longest consecutive sequence path is 3-4-5, so return 3.
```
Example 2:
```
Input:

   2
    \
     3
    / 
   2    
  / 
 1

Output: 2 

Explanation: Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.
```
# 题意
一个二叉树，找到最长的连续顺序的路径。


# 看答案
```c++
class Solution {
public:
	int helper(TreeNode* root, int len){
		if (!root) return len;
		int l = len, r = len;
		if (root->left){
			if (root->left->val == root->val+1)
				l = max(l,helper(root->left, len+1));
			else
				l = max(l, helper(root->left, 1));
		}

		if ( root->right ){
			if (root->right->val == root->val+1)
				r = (r, helper(root->right, len+1));
			else
				r = (r, helper(root->right, 1));
		}
		return max(l, r);
	}
    int longestConsecutive(TreeNode* root) {
  		if (!root) return 0;
  		TreeNode* temp = root;
  		int res = helper(root, 1);
  		return res;
    }
};
```