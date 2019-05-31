---
layout: post
title:  "979. Distribute Coins in Binary Tree"
date: 2019-05-21 22:11:00 -0400
categories: articles
---
Given the root of a binary tree with N nodes, each node in the tree has node.val coins, and there are N coins total.

In one move, we may choose two adjacent nodes and move one coin from one node to another.  (The move may be from parent to child, or from child to parent.)

Return the number of moves required to make every node have exactly one coin.

Example 1:
```
Input: [3,0,0]
Output: 2
Explanation: From the root of the tree, we move one coin to its left child, and one coin to its right child.
```
Example 2:
```
Input: [0,3,0]
Output: 3
Explanation: From the left child of the root, we move two coins to the root [taking two moves].  Then, we move one coin from the root of the tree to the right child.
````
Example 3:
```
Input: [1,0,2]
Output: 2
```
Example 4:
```
Input: [1,0,0,null,3]
Output: 4
```
Note:
```
1<= N <= 100
0 <= node.val <= N
```

```c++
class Solution {
public:
    int res = 0;
    int distributeCoins(TreeNode* root) {
        traversal(root);
        return res;
    }
    int traversal(TreeNode* root)
    {
        if(root->left)
            root->val += traversal(root->left);
        if(root->right)
            root->val += traversal(root->right);
        int temp = root->val -1;
        res += abs(temp);
        return temp;
            
    }
};
```
```c++
class Solution {
public:
    
    int findMovesReq(TreeNode* root, int& moves_req) {
        if(!root)
            return 0;

        int left_excess = findMovesReq(root->left, moves_req);
        int right_excess = findMovesReq(root->right, moves_req);

        // For this subtree, the no. of excess coins at this root 
        // decides the no. of moves req for moving them
        moves_req += abs(left_excess) + abs(right_excess);

        // the excess coins at the root will be the excess number for its parent
        return root->val + left_excess + right_excess - 1;
    }

    int distributeCoins(TreeNode* root) {
        int moves_req = 0;
        findMovesReq(root, moves_req);

        return moves_req;
    }
};
```