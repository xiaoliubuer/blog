---
layout: post
title:  "337. House Robber III"
date: 2019-07-30 20:06:00 -0400
categories: articles
---

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

Example 1:
```
Input: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

Output: 7 
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```
Example 2:
```
Input: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```
```c++
class Solution {
public:
    int rob(TreeNode* root) {
        int robbed, notRobbed;
        helper(root, robbed, notRobbed);
        return max(robbed, notRobbed);
    }

    void helper(TreeNode* root, int& robbed, int& notRobbed) {
        if (!root) {
            robbed = notRobbed = 0;
            return;
        }
        int rRobbed, rNotRobbed, lRobbed, lNotRobbed;

        helper(root->left, lRobbed, lNotRobbed);
        helper(root->right, rRobbed, rNotRobbed);

        robbed = root->val + lNotRobbed + rNotRobbed;
        notRobbed = max(lRobbed, lNotRobbed) + max(rRobbed, rNotRobbed);
    }
};
```
```c++
class Solution {
public:
    int rob(TreeNode* root) {
        return robDFS(root).second;
    }
    pair<int, int> robDFS(TreeNode* node){
        if( !node) return make_pair(0,0);
        auto l = robDFS(node->left);
        auto r = robDFS(node->right);
        int f2 = l.second + r.second;
        int f1 = max(f2, l.first + r.first + node->val);
        return make_pair(f2, f1);
    }
};
```
```c++
class Solution {
public:
  vector<int> robSub(TreeNode* root) {
    vector<int> res(2);
    if (!root) return res;
    
    vector<int> left = robSub(root->left);
    vector<int> right = robSub(root->right);

    res[0] = max(left[0], left[1]) + max(right[0], right[1]);
    res[1] = root->val + left[0] + right[0];
    
    return res;
}
    int rob(TreeNode* root) {
    vector<int> res = robSub(root);
    return max(res[0], res[1]);
}
};
```
```c++
class Solution {
public:
    int tryRob(TreeNode* root, int& l, int& r) {
        if (!root)
            return 0;
            
        int ll = 0, lr = 0, rl = 0, rr = 0;
        l = tryRob(root->left, ll, lr);
        r = tryRob(root->right, rl, rr);
        
        return max(root->val + ll + lr + rl + rr, l + r);
    }

    int rob(TreeNode* root) {
        int l, r;
        return tryRob(root, l, r);
    }
};
```