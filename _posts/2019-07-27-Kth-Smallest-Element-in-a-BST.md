---
layout: post
title:  "230. Kth Smallest Element in a BST"
date: 2019-07-27 19:41:00 -0400
categories: articles
---
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note: 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

Example 1:
```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```
Example 2:
```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```
Follow up:
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

```c++
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        int count = countNodes( root -> left );
        if( k <= count ){
            return kthSmallest( root -> left, k);
        }
        else if ( k > count + 1)
        return kthSmallest( root -> right, k-1-count );
    
        return root->val;
    }
    
    int countNodes(TreeNode* n){
        if( NULL == n) return 0;
        return 1 + countNodes( n -> left ) + countNodes( n -> right );
    }
};
```