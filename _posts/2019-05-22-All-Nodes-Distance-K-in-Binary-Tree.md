---
layout: post
title:  "863. All Nodes Distance K in Binary Tree"
date: 2019-05-22 21:34:00 -0400
categories: articles
---

We are given a binary tree (with root node root), a target node, and an integer value K.

Return a list of the values of all nodes that have a distance K from the target node.  The answer can be returned in any order.

Example 1:
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2

Output: [7,4,1]

Explanation: 
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.
```
Note that the inputs "root" and "target" are actually TreeNodes.
The descriptions of the inputs above are just serializations of these objects.
Note:
```
The given tree is non-empty.
Each node in the tree has unique values 0 <= node.val <= 500.
The target node is a node in the tree.
0 <= K <= 1000.
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
    
    void subtree(vector<int>& dis, unordered_set<TreeNode*>& v, TreeNode* t, int K){
        if(!t || v.count(t)!=0 )return;
        v.insert(t);
        if(K==0){
            dis.push_back(t->val);
            return;
        }
        subtree(dis,v,t->left,K-1);
        subtree(dis,v,t->right,K-1);
    }
    
    void goUp(vector<int>& dis, unordered_map<TreeNode*,TreeNode*>& mp, unordered_set<TreeNode*>& vis, int K,TreeNode* t){
        if(!t || vis.count(t)!=0)return;
        subtree(dis,vis,t,K);
        goUp(dis,mp,vis,K-1,mp[t]);
    }
    
    vector<int> distanceK(TreeNode* root, TreeNode* target, int K) {
        vector<int> dis;
        
        unordered_map<TreeNode*, TreeNode*> parents;
        trav(root,nullptr,parents);
        
        unordered_set<TreeNode*> vis;
        goUp(dis,parents,vis,K,target);
        
        return dis;
        
    }
    
    void trav(TreeNode* root,TreeNode* par,unordered_map<TreeNode*,TreeNode*>& mp){
        if(!root)return;
        mp[root]=par;
        trav(root->left,root,mp);
        trav(root->right,root,mp);
    }
    
};
```