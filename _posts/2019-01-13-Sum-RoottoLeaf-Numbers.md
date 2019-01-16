---
layout: post
title:  "129. Sum Root to Leaf Numbers"
date: 2019-01-13 11:19:23 -0400
categories: articles
---

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

Note: A leaf is a node with no children.

Example:
```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```
Example 2:
```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```
# Signature funciton
```c++
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        
    }
};
```
# 题意
就是对于一个二叉树，从root到leaf的数字属于一个数。返回所有数字的总和。
# 尝试解解
```c++
// Accepted!!
class Solution {
public:
	void helper(TreeNode* root, int currSum, int& res){
		if (!root) return;
		currSum = currSum*10 + root->val;
		if (root && !root->left && !root->right){
			res += currSum;
		}
		helper(root->left, currSum, res);
		helper(root->right,currSum, res);
	}
    int sumNumbers(TreeNode* root) {
    	if (!root) return 0;
    	int res = 0;
    	helper(root, 0, res);
    	return res;
    }
};
```

