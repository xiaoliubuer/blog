---
layout: post
title:  "654. Maximum Binary Tree"
date: 2019-04-09 20:57:00 -0400
categories: articles
---
Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:
```
The root is the maximum number in the array.
The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.
```
Construct the maximum tree by the given array and output the root node of this tree.

Example 1:
```
Input: [3,2,1,6,0,5]
Output: return the tree root node representing the following tree:

      6
    /   \
   3     5
    \    / 
     2  0   
       \
        1
```
Note:
The size of the given array will be in the range [1,1000].

```c++
class Solution {
public:
    
    TreeNode* helper (vector<int>& nums, int left, int right){
        if (left > right) return nullptr;
        int maxVal = nums[left];
        int idx = left;
        for (int i = left; i <= right; i++){
            if ( nums[i] > maxVal ){
                maxVal = nums[i];
                idx = i;
            }
        }
        
        TreeNode* root = new TreeNode(maxVal);
        root->left = helper(nums, left, idx-1);
        root->right= helper(nums, idx+1, right);
        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return helper(nums, 0, nums.size()-1);
    }
};
```