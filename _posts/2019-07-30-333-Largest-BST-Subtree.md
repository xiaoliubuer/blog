---
layout: post
title:  "333. Largest BST Subtree"
date: 2019-07-30 19:56:00 -0400
categories: articles
---
Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

Note:
A subtree must include all of its descendants.

Example:
```
Input: [10,5,15,1,8,null,7]

   10 
   / \ 
  5  15 
 / \   \ 
1   8   7
```
Output: 3
```
Explanation: The Largest BST Subtree in this case is the highlighted one.
             The return value is the subtree's size, which is 3.
```
Follow up:
Can you figure out ways to solve it with O(n) time complexity?

```c++
class Solution {
public:
    int largestBSTSubtree(TreeNode* root) {
        int res = 0, mn = INT_MIN, mx = INT_MAX;
        isValidBST(root, mn, mx, res);
        return res;
    }
    void isValidBST(TreeNode* root, int& mn, int& mx, int& res) {
        if (!root) return;
        int left_cnt = 0, right_cnt = 0, left_mn = INT_MIN;
        int right_mn = INT_MIN, left_mx = INT_MAX, right_mx = INT_MAX;
        isValidBST(root->left, left_mn, left_mx, left_cnt);
        isValidBST(root->right, right_mn, right_mx, right_cnt);
        if ((!root->left || root->val > left_mx) && (!root->right || root->val < right_mn)) {
            res = left_cnt + right_cnt + 1;
            mn = root->left ? left_mn : root->val;
            mx = root->right ? right_mx : root->val;
        } else {
            res = max(left_cnt, right_cnt);    
        }
    }
};
```
```c++
class Solution {
public:
    int largestBSTSubtree(TreeNode* root) {
        int count = 0;
        int left, right;
        countBST(root, left, right, count);
        return count;
    }
    
    int countBST(TreeNode* root, int& left, int& right, int& count) {
        if (root == nullptr) return 0;
        int left1, right1, left2, right2;
        int l = countBST(root->left, left1, right1, count);
        int r = countBST(root->right, left2, right2, count);
        if (l == -1 || r == -1) return -1;
        if ((l == 0 || right1 < root->val) && (r == 0 || left2 > root->val)) {
            count = max(count, l+r+1);
            left = l == 0 ? root->val : left1;
            right = r == 0 ? root->val : right2;
            return l+r+1;
        }
        return -1;
    }
};
```
```c++
class RESULT{
    public:
    int size;
    int lower;
    int upper;
    
    RESULT(int s,int l, int u){
        size=s;
        lower=l;
        upper=u;
    }
}
;
class Solution {
public:
    int MAX;
    int largestBSTSubtree(TreeNode* root) {
    MAX=0;
        if(root==0) return 0;
       reverse(root); 
       return MAX; 
    }
    RESULT reverse(TreeNode* node){
        
        if(node==NULL) {
        RESULT temp(0,INT_MAX,INT_MIN);
            return temp;    
        }
        RESULT left=reverse(node->left);
        RESULT right=reverse(node->right);
        
        if(left.size==-1 ||right.size==-1||node->val<=left.upper|| node->val>=right.lower){
             RESULT temp (-1,0,0);
            return temp;
        }
        int size=left.size+1+right.size;
        MAX=max(size,MAX);
         RESULT temp (size,min(left.lower,node->val),max(right.upper,node->val));
        return temp;
        
        
        
    }
    
};
```
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int largestBSTSubtree(TreeNode* root) {
        int res = 0, minVal = INT_MIN, maxVal=INT_MAX;
        largestBSTSubtreeUtil(root, res, minVal, maxVal);
        return res;
    }
    
private:
    bool largestBSTSubtreeUtil(TreeNode* root, int& res, int& minVal, int& maxVal){
        if(root == nullptr)
            return true;
        
        int lminVal=INT_MIN, lmaxVal=INT_MAX, lcount=0;
        bool left = largestBSTSubtreeUtil(root->left, lcount, lminVal, lmaxVal);
        
        int rminVal=INT_MIN, rmaxVal=INT_MAX, rcount=0;    
        bool right = largestBSTSubtreeUtil(root->right, rcount, rminVal, rmaxVal);
        
        if(left && right){
            if((root->left == nullptr || root->val > lmaxVal) && (root->right == nullptr || root->val < rminVal)){
                res = lcount + 1 + rcount;
                minVal = root->left ? lminVal : root->val;
                maxVal = root->right ? rmaxVal : root->val;
                return true;
            }
        }
        
        res = max(lcount, rcount);
        return false;
    }
};
```