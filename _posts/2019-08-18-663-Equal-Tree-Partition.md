---
layout: post
title:  "663. Equal Tree Partition"
date: 2019-08-18 16:24:00 -0400
categories: articles
---
Given a binary tree with n nodes, your task is to check if it's possible to partition the tree to two trees which have the equal sum of values after removing exactly one edge on the original tree.

Example 1:
```
Input:     
    5
   / \
  10 10
    /  \
   2   3

Output: True
```
Explanation: 
```
    5
   / 
  10
      
Sum: 15

   10
  /  \
 2    3

Sum: 15
```
Example 2:
```
Input:     
    1
   / \
  2  10
    /  \
   2   20

Output: False
```
Explanation: 
```
You can't split the tree into two trees with equal sum after removing exactly one edge on the tree.
```
Note:
```
The range of tree node value is in the range of [-100000, 100000].
1 <= n <= 10000
```

```c++
class Solution {
public:
    unordered_map<int, bool> mp;
    
    int rec(TreeNode *root, TreeNode *cur) {
        if (!cur)
            return 0;
        
        int l = rec(root, cur->left);
        int r = rec(root, cur->right);
        
        if (root != cur)
            mp[l + r + cur->val] = true;
        
        return l + r + cur->val;
    }
    
    bool checkEqualTree(TreeNode* root) {
        int sum = rec(root, root);
        
        if (sum % 2)
            return false;
        
        return mp.find(sum / 2) != mp.end();
    }
};
```
```c++
class Solution {
public:
    bool checkEqualTree(TreeNode* root) {
       int sum = TreeSum(root);
        if (sum % 2 != 0) {
            return false;
        }
       int half_sum = sum/2;
       SubTreeSum(root, root, half_sum);
       return has_equal_tree_;
    }
    
    int TreeSum(TreeNode* root) {
       if (root == nullptr) {
           return 0;
       } 
       return TreeSum(root->left) + TreeSum(root->right) + root->val;
    }
    
    int SubTreeSum(TreeNode* root, TreeNode* node, int sum) {
        if (node == nullptr) {
            return 0;
        }
        int subsum = SubTreeSum(root, node->left, sum) + SubTreeSum(root, node->right, sum) + node->val;
        if (subsum == sum && root != node) {
            has_equal_tree_ = true;
        }
        return subsum;
    }
    
private:
    bool has_equal_tree_ = false;
};
```
```c++
class Solution {
public:
    
    int checkEqual(TreeNode *root, int sum, bool &check)
    {
        if(root == NULL) return 0;;
        int curSum = root->val + checkEqual(root->left, sum, check) + checkEqual(root->right, sum, check);
        if(curSum == sum) check = true;
        return curSum;
    }
    
    bool checkEqualTree(TreeNode* root) {
        int totalSum = total(root);
        if(totalSum & 1) return false;
        
        bool leftCheck = false;
        bool rightCheck = false;
        
        int leftCheckSum = checkEqual(root->left, totalSum/2, leftCheck);
        int rightCheckSum = checkEqual(root->right, totalSum/2, rightCheck);
        
        return leftCheck || rightCheck;
        
    }
    
    int total(TreeNode *root)
    {
        if(root == NULL) return  0;
        return root->val + total(root->left) + total(root->right);
    }
};
```
```c++
class Solution {
public:
    bool checkEqualTree(TreeNode* root) {
        bool res = 0;
        traverse(root,res,0);
        return res;
    }
    int get_child_sum(TreeNode* node){
        if(!node) return 0;
        int left_sum = get_child_sum(node->left);
        int right_sum = get_child_sum(node->right);
        return node->val+left_sum+right_sum;
    }
    void traverse(TreeNode* node, bool& res,int sum){
        if(!node) return;
        sum += node->val;
        int left_sum = get_child_sum(node->left);
        int right_sum = get_child_sum(node->right);
        if((left_sum + sum == right_sum) && node->right) 
			res = 1;
        else if((right_sum + sum == left_sum) && node->left) 
			res = 1;
        traverse(node->left,res,right_sum+sum);
        traverse(node->right,res,left_sum+sum);
    }
};
```
```c++
class Solution {
public:
    int checkEqualTreeUtil(TreeNode* root, TreeNode* node, unordered_set<int>& values) {
        if (!node) return 0;
        
        int value = node->val;
        value += checkEqualTreeUtil(root, node->left, values);
        value += checkEqualTreeUtil(root, node->right, values);
        
        if (node != root) {
            values.insert(value);
        }
        return value;
    }
    
    bool checkEqualTree(TreeNode* root) {
        unordered_set<int> values;
        int total = checkEqualTreeUtil(root, root, values);
        
        if (total % 2) return false;
        return values.count(total / 2);
    }
};
```