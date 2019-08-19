---
layout: post
title:  "655. Print Binary Tree"
date: 2019-08-18 15:53:00 -0400
categories: articles
---

Print a binary tree in an m * n 2D string array following these rules:

The row number m should be equal to the height of the given binary tree.
The column number n should always be an odd number.
The root node's value (in string format) should be put in the exactly middle of the first row it can be put. The column and the row where the root node belongs will separate the rest space into two parts (left-bottom part and right-bottom part). You should print the left subtree in the left-bottom part and print the right subtree in the right-bottom part. The left-bottom part and the right-bottom part should have the same size. Even if one subtree is none while the other is not, you don't need to print anything for the none subtree but still need to leave the space as large as that for the other subtree. However, if two subtrees are none, then you don't need to leave space for both of them.
Each unused space should contain an empty string "".
Print the subtrees following the same rules.
Example 1:
```
Input:
     1
    /
   2
Output:
[["", "1", ""],
 ["2", "", ""]]
```
Example 2:
```
Input:
     1
    / \
   2   3
    \
     4
Output:
[["", "", "", "1", "", "", ""],
 ["", "2", "", "", "", "3", ""],
 ["", "", "4", "", "", "", ""]]
```
Example 3:
```
Input:
      1
     / \
    2   5
   / 
  3 
 / 
4 
Output:

[["",  "",  "", "",  "", "", "", "1", "",  "",  "",  "",  "", "", ""]
 ["",  "",  "", "2", "", "", "", "",  "",  "",  "",  "5", "", "", ""]
 ["",  "3", "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]
 ["4", "",  "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]]
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
    typedef TreeNode* btptr;
    int height(btptr t)
    {
        if(t==NULL)
            return 0;
        return 1+max(height(t->left),height(t->right));
    }
    void preorder(btptr root,vector<vector<string>>&res,int st,int en,int i)
    {
        if(root)
        {
            int mid=(st+(en-st)/2);// as said in question therefore mid is used
            res[i][mid]=to_string(root->val);
            preorder(root->left,res,st,mid,i+1);// next row and st starting and ending index 
            preorder(root->right,res,mid+1,en,i+1);
        }
    }
    vector<vector<string>> printTree(TreeNode* root) 
    {
        int rows=height(root);
        int col=pow(2,rows)-1;// comes from observation of same as of full binary tree
        vector<vector<string>>res(rows,vector<string>(col,""));
        preorder(root,res,0,col-1,0);
        return res;
    }
};
```