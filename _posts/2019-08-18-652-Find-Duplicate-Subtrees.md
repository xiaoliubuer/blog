---
layout: post
title:  "652. Find Duplicate Subtrees"
date: 2019-08-18 15:49:00 -0400
categories: articles
---
Given a binary tree, return all duplicate subtrees. For each kind of duplicate subtrees, you only need to return the root node of any one of them.

Two trees are duplicate if they have the same structure with same node values.

Example 1:
```
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
The following are two duplicate subtrees:

      2
     /
    4
and

    4
Therefore, you need to return above trees' root in the form of a list.
```

```c++
class Solution {
public:
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        unordered_map<long, pair<int,int>> counts; // key, {Id, count}
        vector<TreeNode*> ans;
        getId(root, counts, ans);
        return ans;
    }
private:
    int getId(TreeNode* root, unordered_map<long, pair<int,int>>& counts, vector<TreeNode*>& ans) {
        if(!root) return 0;
        long key = (static_cast<long>(static_cast<unsigned>(root->val))<< 32 ) + 
                   ( getId(root->left, counts, ans) << 16) +
                   ( getId(root->right, counts, ans) );
        auto& p = counts[key];
        if (p.second++ == 0)
            p.first = counts.size();  // assign a new Id   
        else if (p.second == 2) // this Id appeared for the second time
            ans.push_back(root);
        return p.first;                   
    }
};
```
```c++
void find_dup(TreeNode* root, 
              map<tuple<int, TreeNode*, TreeNode*>, TreeNode*> &m, 
              unordered_map<TreeNode*, TreeNode*> &inv_map,
              unordered_set<TreeNode*> &s)
{
    if (root == NULL)
        return;
    
    find_dup(root->left, m, inv_map, s);
    find_dup(root->right, m, inv_map, s);
    
    TreeNode* left = inv_map[root->left];
    TreeNode* right = inv_map[root->right];
    
    tuple<int, TreeNode*, TreeNode*> key = {root->val, left, right};
    auto it = m.find(key);
    if (it == m.end()) {
        m.insert({key, root});
        inv_map[root] = root;
    } else {
        s.insert(it->second);
        inv_map[root] = it->second;
    }
}

class Solution {
public:
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        map<tuple<int, TreeNode*, TreeNode*>, TreeNode*> m;
        unordered_map<TreeNode*, TreeNode*> inv_map;
        unordered_set<TreeNode*> s;
        
        find_dup(root, m, inv_map, s);
        
        vector<TreeNode*> result;
        for (auto &k: s)
            result.push_back(k);
        return result;
    }
};
```
```c++

void find_dup(TreeNode* root, 
              map<tuple<int, TreeNode*, TreeNode*>, TreeNode*> &m, 
              unordered_map<TreeNode*, TreeNode*> &inv_map,
              set<TreeNode*> &s)
{
    if (root == NULL)
        return;
    
    find_dup(root->left, m, inv_map, s);
    find_dup(root->right, m, inv_map, s);
    
    TreeNode* left = inv_map[root->left];
    TreeNode* right = inv_map[root->right];
    
    tuple<int, TreeNode*, TreeNode*> key = {root->val, left, right};
    auto it = m.find(key);
    if (it == m.end()) {
        m.insert({key, root});
        inv_map[root] = root;
    } else {
        s.insert(it->second);
        inv_map[root] = it->second;
    }
}

class Solution {
public:
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        map<tuple<int, TreeNode*, TreeNode*>, TreeNode*> m;
        unordered_map<TreeNode*, TreeNode*> inv_map;
        set<TreeNode*> s;
        
        find_dup(root, m, inv_map, s);
        
        vector<TreeNode*> result;
        for (auto &k: s)
            result.push_back(k);
        return result;
    }
};
```

```c++
class Solution {
public:
    string findDuplicate(TreeNode* root, unordered_map<string, TreeNode*>& mp, set<TreeNode*>& ret) {
        if(!root)
            return "$ ";
        string s = "$" + to_string(root -> val) + findDuplicate(root -> left, mp, ret) + findDuplicate(root -> right, mp, ret);
        
        if(mp.find(s) != mp.end()) {
            ret.insert(mp[s]);
        }
        else {
            mp[s] = root;
        }
        return s;
    }
    
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        unordered_map<string, TreeNode*> mp;
        set<TreeNode*> ret;
        findDuplicate(root, mp, ret);
        return vector<TreeNode*>(ret.begin(), ret.end());
    }
};
```
```c++
class Solution {
private:
    string findDuplicateTrees( TreeNode* root, vector< TreeNode* >& res, unordered_map< string, int >& counter ){
        if ( ! root )
            return "n";
        string leftPart = findDuplicateTrees( root -> left, res, counter );
        string rightPart = findDuplicateTrees( root -> right, res, counter );
        string fullTree = to_string( root -> val ) + "," + leftPart + "," + rightPart;
        counter[ fullTree ] ++;
        if( counter[ fullTree ] == 2 )
            res.push_back( root );
        return fullTree;
    }
public:
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        if( ! root )
            return vector< TreeNode* >();
        unordered_map< string, int > counter;
        vector< TreeNode* > res;
        findDuplicateTrees( root, res, counter );
        return res;
    }
};
```