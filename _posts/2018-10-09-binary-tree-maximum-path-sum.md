---
layout: post
title:  "124. Binary Tree Maximum Path Sum"
date: 2018-10-09 20:27:23 -0400
categories: articles
---
Given a non-empty binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.

Example 1:

Input: [1,2,3]
```
       1
      / \
     2   3
```
Output: 6
Example 2:

Input: [-10,9,20,null,null,15,7]
```
  -10
   / \
  9  20
    /  \
   15   7
```
Output: 42

思路:
一个非空2叉树，找到最大的path sum, 就是从任意node到任意node。这个就有意思啦。有负数存在，那怎么办？
我操，完全没思路呀。

因为只能从根节点出发，一个个往下顺，假如只有一个节点，那么就是-10，但如果它有两个子，那么就要比较左，右，
左+中，右+中，左+中+右。

由上面，我们进行提炼，

也就是
如果 root 大于 0， 如果 左右都小于0， 直接返回 root，
如果都大于0，返回 左 + 中 + 右
如果有一方小于0，那么就返回 另一方 + 中。

如果 root 小于 0，
都小于0，返回 root
都 大于0， 返回左 + 中 + 右
以防小于0， 返回 另一方 

所以我们在每一层会有两个值，一个是每一层当前最大值，要有就是本层要返回的最大值。

但是会发现，


现在要考虑的是什么时候只取单边，什么时候要过root。

左，右，中 都大于0， 当然要过。


如果左，右都大于0，

```c++
//Answer
class Solution {
public:
    
    int helper(TreeNode* root, int& max_val){
        if (!root) return 0;
        
        int max_left = helper(root->left, max_val);
        int max_right= helper(root->right,max_val);
        
        if (max_left < 0) max_left = 0;
        if (max_right< 0) max_right = 0;
        
        if (max_left + max_right + root->val > max_val) max_val = (max_left + max_right + root->val);
        return max(max_left, max_right) + root->val;
        
        
    }
    int maxPathSum(TreeNode* root) {
        if (!root) return 0;
        int max_val = INT_MIN;
        helper(root, max_val);
        return max_val;
    }
};
```
其实就是挺简单的，咋啦就是没做对呢？？？？

```c++
//Accepted!!
class Solution {
public:
    int res = INT_MIN;
    int helper (TreeNode* root){
        if (!root) return -99;
        int left_val = helper(root->left);
        int right_val = helper(root->right);
        
        int side = max(left_val, right_val);
        int cross = left_val + right_val + root->val;
        int level = max(side, max(side + root->val, cross));
        res = max(res, level);
        return max(root->val,side + root->val);
    }
    
    
    int maxPathSum(TreeNode* root) {
        if (!root) return 0;
        return max(res,helper(root));
    }
};
```
