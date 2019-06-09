---
layout: post
title:  "108. Convert Sorted Array to Binary Search Tree"
date: 2019-06-04 22:08:00 -0400
categories: articles
---

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example:
```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```
```c++
/*
	TC:
	SC:
*/
class Solution {
    TreeNode* CreateSection(vector<int>& nums,int l,int r){
        if(l>r) return NULL;
        int c = (l+r+1)/2;
        TreeNode *root = new TreeNode(nums[c]);
        root->left = CreateSection(nums,l,c-1);
        root->right = CreateSection(nums,c+1,r);
        return root;
    }
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return CreateSection(nums,0,nums.size()-1);
    }
};
```