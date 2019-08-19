---
layout: post
title:  "662. Maximum Width of Binary Tree"
date: 2019-08-18 16:19:00 -0400
categories: articles
---
Given a binary tree, write a function to get the maximum width of the given tree. The width of a tree is the maximum width among all levels. The binary tree has the same structure as a full binary tree, but some nodes are null.
The width of one level is defined as the length between the end-nodes (the leftmost and right most non-null nodes in the level, where the null nodes between the end-nodes are also counted into the length calculation.

Example 1:
```
Input: 

           1
         /   \
        3     2
       / \     \  
      5   3     9 

Output: 4
Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).
```
Example 2:
```
Input: 

          1
         /  
        3    
       / \       
      5   3     

Output: 2
Explanation: The maximum width existing in the third level with the length 2 (5,3).
```
Example 3:
```
Input: 

          1
         / \
        3   2 
       /        
      5      

Output: 2
Explanation: The maximum width existing in the second level with the length 2 (3,2).
```
Example 4:
```
Input: 

          1
         / \
        3   2
       /     \  
      5       9 
     /         \
    6           7
Output: 8
Explanation:The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).
```

Note: Answer will in the range of 32-bit signed integer.


```c++
class Solution {

public:
    int widthOfBinaryTree(TreeNode* root) {
        vector<pair<long, TreeNode *>> value = vector<pair<long, TreeNode *>>();
        vector<pair<long, TreeNode *>> temp = vector<pair<long, TreeNode *>>();
        value.push_back({1, root});
        
        int ret = 0, start;
        while (value.size() > 0) {
            ret = max(ret, (int)(value[value.size()-1].first - value[0].first + 1));
            start = value[0].first;
            
            temp.clear();
            for (auto p : value) {
                if (p.second->left != NULL) 
                    temp.push_back({(p.first-start)*2, p.second->left});
                if (p.second->right != NULL)
                    temp.push_back({(p.first-start)*2+1, p.second->right});
            }
            
            value.clear();
            for (auto p : temp) value.push_back(p);
        }
        
        return ret;
    }
};
```
```c++
class Solution {
public:
    int widthOfBinaryTree(TreeNode* root) {
        int maxWidth = 0;
        queue<NodePosition> q;
        if (root) {
            q.push(std::move(NodePosition(root, 0)));
        }
        
        while (!q.empty()) {
            int size = q.size();
            int minPosition = q.front().position;
            int maxPosition = q.front().position;
            
            for (int i = 0; i < size; ++i) {
                auto tmp = q.front();
                q.pop();
                
                minPosition = min(minPosition, tmp.position);
                maxPosition = max(maxPosition, tmp.position);
                
                if (tmp.node->left) {
                    q.push(std::move(NodePosition(tmp.node->left, tmp.position*2 - minPosition)));
                }
                
                if (tmp.node->right) {
                    q.push(std::move(NodePosition(tmp.node->right, tmp.position*2+1 - minPosition)));
                }
            }
            maxWidth = max(maxWidth, maxPosition - minPosition + 1);
        }
        return maxWidth;
    }
};
```
```c++
class Solution {
    unsigned int width(TreeNode *r, unsigned int id, int d, vector<int> &left)
    {
        if(!r) return 0;
        
        if(d >= left.size()) left.push_back(id);
        return max(
            {id + 1 - left[d], 
             width(r->left, id * 2, d + 1, left), 
             width(r->right, id * 2 + 1, d + 1, left)});
    }
public:
    int widthOfBinaryTree(TreeNode* root) {
        vector<int> left;
        int out = width(root, 1, 0, left);
        return out;
    }
};
```
```c++
class Solution {
public:
    vector<int> left, right;
    void FindWidth(TreeNode *root, long long node_num, int level) {
        if (root == nullptr)
            return;
        if (level >= left.size()) {
            left.resize(level + 1);
            right.resize(level + 1);
            left[level] = node_num;
            right[level] = node_num;
        }
        if (node_num < left[level])
            left[level] = node_num;
        if (node_num > right[level])
           right[level] = node_num;
        if (root->left != nullptr)
            FindWidth(root->left, 2 * node_num + 1, level + 1);
        if (root->right != nullptr)
            FindWidth(root->right, 2 * node_num + 2, level + 1);
    }
    int widthOfBinaryTree(TreeNode* root) {
        if (root == nullptr)
            return 0;
        if (root->left == nullptr && root->right == nullptr)
            return 1;
        if (root->right == nullptr)
            return widthOfBinaryTree(root->left);
        if (root->left == nullptr)
            return widthOfBinaryTree(root->right);
        FindWidth(root, 0, 0);
        int max_width = 1;
        for (int i = 0; i < left.size(); ++i) {
            int w = right[i] + 1 - left[i];
            if (w > max_width)
                max_width = w;
        }
        return max_width;
    }
};
```