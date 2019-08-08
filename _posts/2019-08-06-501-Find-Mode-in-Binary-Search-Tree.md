---
layout: post
title:  "501. Find Mode in Binary Search Tree"
date: 2019-08-06 21:19:00 -0400
categories: articles
---
Given a binary search tree (BST) with duplicates, find all the mode(s) (the most frequently occurred element) in the given BST.

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than or equal to the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.
 

For example:
```
Given BST [1,null,2,2],

   1
    \
     2
    /
   2
 

return [2].
```
Note: If a tree has more than one mode, you can return them in any order.
```
Follow up: Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).
```
```c++
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
        if (root == NULL)
            return vector<int> ();
        
        unordered_map<int, int> track_count;
        populate_map (track_count, root);
        
		// Find the maximum occurence
        int max = INT_MIN;
        unordered_map<int, int>:: iterator i;
        for (i = track_count.begin(); i != track_count.end(); i++)
            if (i->second > max)
                max = i->second;
        
		// Loop through the map again to add numbers with max count to vector 
        vector<int> res;
        for (i = track_count.begin(); i != track_count.end(); i++)
            if (i->second == max)
                res.push_back(i->first);
        return res;
    }
    
    void populate_map (unordered_map<int, int>& map, TreeNode* root){
        if (root == NULL)
            return;
        if (map.count(root->val) == 0)
            map[root->val] = 1;
        else
            map[root->val]++;
        populate_map (map, root->left);
        populate_map (map, root->right);
    }
};
```