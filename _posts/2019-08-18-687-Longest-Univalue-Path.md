---
layout: post
title:  "687. Longest Univalue Path"
date: 2019-08-18 17:52:00 -0400
categories: articles
---
Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.

The length of path between two nodes is represented by the number of edges between them.

Example 1:
```
Input:

              5
             / \
            4   5
           / \   \
          1   1   5
Output: 2
```
Example 2:
```
Input:

              1
             / \
            4   5
           / \   \
          4   4   5
Output: 2
```
Note: The given binary tree has not more than 10000 nodes. The height of the tree is not more than 1000.


```c++
class Solution {
public:
    int m;
    
    int longestUnivaluePath(TreeNode* root) {
        m = 0;
        if(root) LUP(root);
        return m;
    }
    
    int LUP(TreeNode *node) {
        int L = 0, R = 0;
        if(node->left) {            
            L = 1 + LUP(node->left);
            if(node->val != node->left->val) L = 0;
        } 
        if(node->right) {
            R = 1 + LUP(node->right);
            if(node->val != node->right->val) R = 0;
        }
        if(L + R > m) m = L + R;
        return max(L, R);
    }    
};
```

```c++
class Solution {
public:
    int m;
    
    int longestUnivaluePath(TreeNode* root) {
        m = 0;
        LUP(root);
        return m;
    }
    
    int LUP(TreeNode *node) {
        if(node) {
            int L = 1 + LUP(node->left);
            int R = 1 + LUP(node->right);
            if(node->left == NULL || node->val != node->left->val) L = 0;
            if(node->right== NULL || node->val != node->right->val) R = 0;            
            if(L + R > m) m = L + R;
            if(L > R) return L; return R;
        } return 0;
    }    
};
```
```c++
class Solution {
public:
    int m;
    
    int longestUnivaluePath(TreeNode* root) {
        m = 0;
        LUP(root);
        return m;
    }
    
    int LUP(TreeNode *node) {
        if(node) {
            int L = 1 + LUP(node->left);
            int R = 1 + LUP(node->right);
            if(node->left == NULL || node->val != node->left->val) L = 0;
            if(node->right== NULL || node->val != node->right->val) R = 0;            
            if(L + R > m) m = L + R;
            if(L > R) return L; return R;
        } return 0;
    }    
};
```
```c++
class Solution {
public:
    int longestUnivaluePath(TreeNode *root) {
        int ans = 0;
        helper(root, ans);
        return ans;
    }
    int helper(TreeNode* root, int &ans) {
        if (!root) {
            return 0;
        }
        int left = helper(root->left, ans);
        int right = helper(root->right, ans);
        if (!root->left || root->left->val != root->val) {
            left = 0;
        }
        if (!root->right || root->right->val != root->val) {
            right = 0;
        }
        ans = max(ans, left + right);
        return 1 + max(left, right);
    }
};
```
```c++
class Solution {
public:
    int longestUnivaluePath(TreeNode* root) {
        int lup = 0;
        if (root) dfs(root, lup);
        return lup;
    }

private:
    int dfs(TreeNode* node, int& lup) {
        int l = node->left ? dfs(node->left, lup) : 0;
        int r = node->right ? dfs(node->right, lup) : 0;
        int resl = node->left && node->left->val == node->val ? l + 1 : 0;
        int resr = node->right && node->right->val == node->val ? r + 1 : 0;
        lup = max(lup, resl + resr);
        return max(resl, resr);
    }
};
```