---
layout: post
title:  "563. Binary Tree Tilt"
date: 2019-08-11 16:52:00 -0400
categories: articles
---
Given a binary tree, return the tilt of the whole tree.
The tilt of a tree node is defined as the absolute difference between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.

The tilt of the whole tree is defined as the sum of all nodes' tilt.

Example:
```
Input: 
         1
       /   \
      2     3
Output: 1
```
Explanation: 
```
Tilt of node 2 : 0
Tilt of node 3 : 0
Tilt of node 1 : |2-3| = 1
Tilt of binary tree : 0 + 0 + 1 = 1
```
Note:
```
The sum of node values in any subtree won't exceed the range of 32-bit integer.
All the tilt values won't exceed the range of 32-bit integer.
```
```c++
class Solution {
public:
    int findTilt(TreeNode* root) {
        if(!root) return 0;
        int total = 0;
        sum(root,total);
        return total;
        
    }
    int sum(TreeNode* root, int& total) {
        if(!root) return 0;
        int sum1 = sum(root->right, total);
        int sum2 = sum(root->left, total);
        total += abs(sum1-sum2);
        return((root->val)+(sum1+sum2)); 
    }

};
```
```c++
class Solution {
public:
    int helper(TreeNode* root, int& currentTilt) {
        
        if(!root) {
            return 0;
        }
        
        int l = helper(root->left, currentTilt);
        int r = helper(root->right, currentTilt);
        
        currentTilt += std::abs(r-l);
        return l + r + root->val;
        
    }
    
    int findTilt(TreeNode* root) {
        int t = 0;
        helper(root, t);
        return t;
    }
};
```
```c++
class Solution {
public:
    int findTiltOfNode(TreeNode* root, int& re){
        if(root==nullptr)
            return 0;
        int left_sum=findTiltOfNode(root->left, re);
        int right_sum=findTiltOfNode(root->right, re);
        re+=abs(left_sum-right_sum);
        return left_sum+right_sum+root->val;
    }
    int findTilt(TreeNode* root) {
        int re=0;
        findTiltOfNode(root,re);
        return re;
    }
};
```
```c++
class Solution {
public:
    int computeSum(TreeNode* root, vector<int>* tilts) {
        if (!root) return 0;
        int left_sum = computeSum(root->left, tilts);
        int right_sum = computeSum(root->right, tilts);
        tilts->push_back(abs(left_sum-right_sum));
        return left_sum + right_sum + root->val;
    }
    
    int findTilt(TreeNode* root) {
        if (!root) return 0;
        vector<int> tilts;
        computeSum(root, &tilts);
        int sum = 0;
        for (int t : tilts) sum+=t;
        return sum;
    }
};
```
```c++
class Solution {
public:
    int sum(TreeNode *root)
    {
        if(!root)
            return 0;
        return root->val + sum(root->right) + sum(root->left);
    }
    int findTilt(TreeNode* root) {
        if(!root)
            return 0;
        if(!root->left && !root->right)
            return 0;
        int left = findTilt(root->left);
        int right = findTilt(root->right);
       
        int l = sum(root->left);
        int r = sum(root->right);
        
         return left+right + abs(l-r);
    }
};
```
```c++
class Solution {
public:
    int getSum(TreeNode* root) {
        if(!root) return 0;
        return root->val + getSum(root->left) + getSum(root->right);
    }

    int findTilt(TreeNode* root) {
        if(!root) return 0;
        return abs(getSum(root->left) - getSum(root->right)) + findTilt(root->left) + findTilt(root->right);
    }
};
```