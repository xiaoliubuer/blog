---
layout: post
title:  "606. Construct String from Binary Tree"
date: 2019-08-14 19:39:00 -0400
categories: articles
---	

You need to construct a string consists of parenthesis and integers from a binary tree with the preorder traversing way.

The null node needs to be represented by empty parenthesis pair "()". And you need to omit all the empty parenthesis pairs that don't affect the one-to-one mapping relationship between the string and the original binary tree.

Example 1:
Input: Binary tree: [1,2,3,4]
       1
     /   \
    2     3
   /    
  4     

Output: "1(2(4))(3)"

Explanation: Originallay it needs to be "1(2(4)())(3()())", 
but you need to omit all the unnecessary empty parenthesis pairs. 
And it will be "1(2(4))(3)".
Example 2:
Input: Binary tree: [1,2,3,null,4]
       1
     /   \
    2     3
     \  
      4 

Output: "1(2()(4))(3)"

Explanation: Almost the same as the first example, 
except we can't omit the first parenthesis pair to break the one-to-one mapping relationship between the input and the output.
```c+
class Solution {
public:
    string s;
    
    string tree2str(TreeNode* t) {
        s.clear();
        if(t) pre(t);
        return s;
    }
    
    void pre(TreeNode *node) {
        s += to_string(node->val);
        if(node->left) { s += "("; pre(node->left); s += ")"; }
        if(node->right) {
            if(node->left == NULL) s += "()";
            s += "("; pre(node->right); s += ")";
        }
    }
};
```
```c++
class Solution {
public:
    string s;
    
    string tree2str(TreeNode* t) {
        s.clear();
        if(t) pre(t);
        return s;
    }
    
    void pre(TreeNode *node) {
        s += to_string(node->val);
        if(node->left) { s += "("; pre(node->left); s += ")"; }
        if(node->right) {
            if(node->left == NULL) s += "()";
            s += "("; pre(node->right); s += ")";
        }
    }
};
```

```c++
class Solution {
public:
    string tree2str(TreeNode* t) {
        string str;
        if(t==NULL)
            return str;
        helper(t,str);
        return str;
    }
    
    void helper(TreeNode* node,string& str){
        if(node==NULL)
            return;        
        str.append(to_string(node->val));  
        if(node->left!=NULL||node->right!=NULL){ 
            str.append("(");
            helper(node->left,str);
            str.append(")");
        }
        if(node->right!=NULL){
            str.append("(");
            helper(node->right,str);
            str.append(")");
        }
    }
};
```
```c++
class Solution {
public:
    string tree2str(TreeNode* t) {
        if(t==NULL){
            return "";
        }
        
        string res = to_string(t->val);
        
        if(t->left!=NULL || t->right!=NULL){
            res+="(";
            res+=tree2str(t->left);
            res+=")";
        }
        
        if(t->right!=NULL){
            res+="(";
            res+=tree2str(t->right);
            res+=")";
        }   
        
        return res;
    }
};
```