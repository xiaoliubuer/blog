---
layout: post
title:  "572. Subtree of Another Tree"
date: 2019-08-11 17:22:00 -0400
categories: articles
---
Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.

Example 1:
```
Given tree s:

     3
    / \
   4   5
  / \
 1   2
Given tree t:
   4 
  / \
 1   2
Return true, because t has the same structure and node values with a subtree of s.
```
Example 2:
```
Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0
Given tree t:
   4
  / \
 1   2
Return false.
```
```c++
class Solution {
private:
    bool isSame(TreeNode* s, TreeNode* t){
        if(s->val!=t->val)return false;
        if((s->left==NULL?0:1)^(t->left==NULL?0:1)==1){
            return false;
        }
        if((s->right==NULL?0:1)^(t->right==NULL?0:1)==1){
            return false;
        }
        if(s->left&&!isSame(s->left,t->left)){
            return false;
        }
        if(s->right&&!isSame(s->right,t->right)){
            return false;
        }
        return true;
    }
public:
    bool isSubtree(TreeNode* s, TreeNode* t) {
        if(s==NULL&&t==NULL)return true;
        
        if(isSame(s,t)){
            return true;
        }
        else{
            if(s->left&&isSubtree(s->left,t)){
                return true;
            }
            if(s->right&&isSubtree(s->right,t)){
                return true;
            }
            return false;
        }
    }
};
```
```c++
class Solution {
public:
    // pre-order, transoform to substring problem
    bool isSubtree(TreeNode* s, TreeNode* t) {
        string s1,s2;
        preOrder(s,s1);
        preOrder(t,s2);
        return s1.find(s2) != string::npos;
    }
    
    void preOrder(TreeNode* t, string& str)
    {
        if (!t)
        {
            str.push_back('x');
            return;
        }
        
        str.push_back(t->val);
        
        preOrder(t->left,str);
        preOrder(t->right,str);
    }
};
```
```c++
class Solution {
public:
    bool isSubtree(TreeNode* s, TreeNode* t) {
        if (!s) return false;
        if (sameTree(s, t)) return true;
        return isSubtree(s->left, t) || isSubtree(s->right, t);
    }
    
    bool sameTree(TreeNode* s, TreeNode* t) {
        if (!s && !t) return true;
        if (!s || !t) return false;
        if (s->val != t->val) return false;
        return sameTree(s->left, t->left) && sameTree(s->right, t->right);
    }
};
```
```c++
class Solution {
private:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p==NULL && q==NULL)
            return true;
        if(p==NULL || q==NULL)
            return false;
        if(p->val == q->val)
            return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
        else
            return false;
    }
public:
    bool isSubtree(TreeNode* s, TreeNode* t) {
        if(!s) return false;
        return isSameTree(s,t) || isSubtree(s->left,t) || isSubtree(s->right,t);
    }
};
```
```c++
class Solution {
public:
    bool isSameTree(TreeNode* s, TreeNode* t){
        if(s==nullptr&&t==nullptr)
            return true;
        if(s==nullptr||t==nullptr)
            return false;
        return s->val==t->val&&isSameTree(s->left, t->left)&&isSameTree(s->right, t->right);
    }
    bool isSubtree(TreeNode* s, TreeNode* t) {
        bool re=isSameTree(s, t);
        if(s->left)
            re = re||isSubtree(s->left, t);
        if(s->right)
            re = re||isSubtree(s->right, t);
        return re;
    }
};
```
```c++
class Solution {
public:
bool flag;
    bool check(TreeNode *T1, TreeNode *T2) {
        if (T1 == NULL && T2 != NULL)
            return false;
        if (T1 != NULL && T2 == NULL)
            return false;

        if (T1 == NULL && T2 == NULL)
            return true;
        if (T1->val != T2->val)
            return false;
        
        return check(T1->left, T2->left) && check(T1->right, T2->right);
    }

    void dfs(TreeNode *T1, TreeNode *T2) {
        //if (flag) return;
        if (check(T1, T2)) {
            flag = true;
            return;
        }
        if (T1 == NULL)            
            return;
    
        dfs(T1->left, T2);
        dfs(T1->right, T2);
        
    }
    bool isSubtree(TreeNode *T1, TreeNode *T2) {
        // write your code here
        flag = false;
        dfs(T1, T2);
        return flag;
    }
};
```