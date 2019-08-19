---
layout: post
title:  "549. Binary Tree Longest Consecutive Sequence II"
date: 2019-08-09 21:20:00 -0400
categories: articles
---
Given a binary tree, you need to find the length of Longest Consecutive Path in Binary Tree.
Especially, this path can be either increasing or decreasing. For example, [1,2,3,4] and [4,3,2,1] are both considered valid, but the path [1,2,4,3] is not valid. On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

Example 1:
```
Input:
        1
       / \
      2   3
Output: 2
Explanation: The longest consecutive path is [1, 2] or [2, 1].
```

Example 2:
```
Input:
        2
       / \
      1   3
Output: 3
Explanation: The longest consecutive path is [1, 2, 3] or [3, 2, 1].
```
Note: All the values of tree nodes are in the range of [ - 1e7, 1e7 ].
```c++
class Solution {
public:
    int res;
    int longestConsecutive(TreeNode* root) {
        res = 0;
        helper(root);
        return res;
    }
    pair<int,int> helper(TreeNode* root){ //r
        if(!root) return {0,0};
        pair<int,int> left = helper(root->left);
        pair<int,int> right = helper(root->right);
        int inc = 1;
        int dec = 1;
        if(root->left){
            //increasing
            if(root->left->val == root->val + 1) inc = max(inc, left.first + 1);
            //decreasing
            else if(root->left->val == root->val - 1) dec = max(dec, left.second + 1);
        }
        if(root->right){
            if(root->right->val == root->val + 1) inc = max(inc, right.first + 1);
            else if(root->right->val == root->val - 1) dec = max(dec, right.second + 1);
        }
        res = max(res, inc + dec - 1);
        return make_pair(inc, dec);
    }
};
```
```c++
class Solution {
public:
    int longestConsecutive(TreeNode* root) {
        int res = 0;
        helper(root, res);
        return res;
    }
    pair<int, int> helper(TreeNode* root, int &res){
        if(root == nullptr)
            return make_pair(0, 0);
        int max_inc = 1, max_dec = 1;
        if(root->left != nullptr){
            auto p = helper(root->left, res);
            if(root->val == root->left->val + 1){
                max_dec = p.second + 1;
            }else if(root->val == root->left->val - 1){
                max_inc = p.first + 1;
            }
        }
        if(root->right != nullptr){
            auto p = helper(root->right, res);
            if(root->val == root->right->val + 1){
                max_dec = max(max_dec, p.second + 1);
            }else if(root->val == root->right->val - 1){
                max_inc = max(max_inc, p.first + 1);
            }
        }
        res = max(res, max_dec + max_inc - 1);
        return make_pair(max_inc, max_dec);
    }
};
```
```c++
class Solution {
public:
    int helper(TreeNode* root, int prev, int& ans) {
        if (root == NULL) return 0;
        int cur = 0;
        // if 1 smaller than previous num, will return the longest decr path  
        if (root->val == prev-1) cur--;
        // else if 1 larger than previous num, will return the longest incr path
        else if (root->val == prev+1) cur++;
        // else can't be extended so will return 0
        // now find the longest path rooted at this node
        int L = helper(root->left, root->val, ans);
        int R = helper(root->right, root->val, ans);
        // if one is decreasing and one is increasing, then can be combined
        if ((L < 0 && R > 0) || (L > 0 && R < 0)) {
            if (abs(L)+abs(R)+1 > ans) ans = abs(L)+abs(R)+1;
        }
        if (abs(L)+1 > ans) ans = abs(L)+1;
        if (abs(R)+1 > ans) ans = abs(R)+1;
        if (cur == 0) return 0;
        else if (cur > 0) return 1+max(max(L,R),0);
        else return min(min(L,R),0)-1;
        
    }
    
    int longestConsecutive(TreeNode* root) {
        if (root == NULL) return 0;
        else if (root->left == NULL && root->right == NULL) return 1;
        int ans = 1;
        helper(root, root->val+2, ans);
        return ans;
    }
};
```
```c++
class Solution {
public:
    int inc = 0;
    int dec = 0;
    pair<int,int> longestIncDec(TreeNode *root){
        if(!root){
            return {0,0};
        }
        
        auto left = longestIncDec(root->left);
        auto right = longestIncDec(root->right);
        int maxInc = 1, maxDec = 1;
        
        if(left.first && root->left->val + 1 == root->val){
            maxDec = left.first + 1;
            if(right.second && root->right->val == root->val + 1){
                dec = max(dec,maxDec + right.second);
            }
        }
        
        if(left.second && root->left->val == root->val + 1){
            maxInc = left.second + 1;   
        }
        
        if(right.first && root->right->val + 1== root->val){
            maxDec = max(maxDec, right.first + 1);
            if(left.second && root->left->val == root->val + 1){
                dec = max(dec, right.first + 1 + left.second);
            }
        }
        
        if(right.second && root->right->val == root->val + 1){
            maxInc = max(right.second + 1,maxInc);
        }
		
        inc = max(inc,maxInc);
        dec = max(dec,maxDec);
        
        return {maxDec,maxInc};
    }
    
    int longestConsecutive(TreeNode* root) {
        longestIncDec(root);
        return max(inc,dec);
    }
};
```