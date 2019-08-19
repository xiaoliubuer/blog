---
layout: post
title:  "536. Construct Binary Tree from String"
date: 2019-08-07 21:33:00 -0400
categories: articles
---
You need to construct a binary tree from a string consisting of parenthesis and integers.

The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.

You always start to construct the left child node of the parent first if it exists.

Example:
```
Input: "4(2(3)(1))(6(5))"
Output: return the tree root node representing the following tree:

       4
     /   \
    2     6
   / \   / 
  3   1 5   
```
Note:
```
There will only be '(', ')', '-' and '0' ~ '9' in the input string.
An empty tree is represented by "" instead of "()".
```
```c++
class Solution {
public:
    TreeNode* str2tree(string s) {
        int i = 0;
        return s.size() == 0 ? nullptr : build(s, i);
    }

private:
    TreeNode* build(string& s, int& i) {
        int start = i;
        if (s[i] == '-') {
            i++;
        }
        while (isdigit(s[i])) {
            i++;
        }
        
        int num = stoi(s.substr(start, i - start));
        TreeNode* node = new TreeNode(num);
        if (s[i] == '(') {
            node->left = build(s, ++i);
            i++;    // )
        }
        if (s[i] == '(') {
            node->right = build(s, ++i);
            i++;    // )
        }
        return node;
    }
};
```

```c++
class Solution {
public:
    TreeNode* str2tree(string s) {
        if(s.length() == 0) return nullptr;
        int index = s.find('(');
        if(index == string::npos){ /* only value case */
            return (new TreeNode(stoi(s)));
        }
        else{ /* combination case, like 2(3)(6) */
            TreeNode* res = new TreeNode(stoi(s.substr(0,index)));
            int parentheses_cnt = 1;
            int start = ++index;
            while(parentheses_cnt){
                if(s[index] == '(') parentheses_cnt++;
                else if(s[index] == ')') parentheses_cnt--;
                index++;
            }//now, index will point to  2(3) -->(<--6)
            res->left = str2tree(s.substr(start,index-start-1));
            if(index != s.length()) // sometimes we don't have right child
                res->right = str2tree(s.substr(index+1,s.length()-index-2));
            return res;
        }
    }
};
```
```c++
class Solution {
public:
    TreeNode* str2tree(string s) {
        if (s.empty()) return nullptr;
        int n = s.size();
        stack<TreeNode*> stk;
        for (int i = 0; i < n; ++i) {
            if (s[i] == '-' || isdigit(s[i])) {
                int len = 1;
                while (i + len < n && isdigit(s[i+len])) len++;
                TreeNode* t = new TreeNode(stoi(s.substr(i, len)));
                stk.push(t);
                i += len - 1;
            } else if (s[i] == ')') {
                TreeNode* t = stk.top(); stk.pop();
                TreeNode* ft = stk.top(); stk.pop();
                if (ft->left) {
                    ft->right = t;
                } else {
                    ft->left = t;
                }
                stk.push(ft);
            }
        }
        return stk.top();
    }
};
```
```c++
class Solution {
public:
    TreeNode* str2tree(string s) {
        if(s.empty()) return NULL;
        int i = 0, n = s.size();
        // read first num
        while(i < n && s[i] != '(' && s[i] != ')')
            i++;
        TreeNode *root = new TreeNode(stoi(s.substr(0, i)));
        //use stack to track parent node
        stack<TreeNode*> st;
        st.push(root); 
        
        while(i < n){
            if(s[i] == '('){
                i++;
                //read num
                string str = "";
                while(s[i] != '(' && s[i] != ')' && i < n){
                    str += s[i]; i++;
                }
                //create node
                TreeNode* temp = new TreeNode(stoi(str));
                //use st to find left or right
                TreeNode* parent =st.top();
                if(parent->left == NULL) parent->left = temp; 
                else parent->right = temp;
                
                st.push(temp);
            }
            if(s[i] == ')'){
                st.pop();
                i++;
            }
        }
        return root;
    }
};
```
```c++
class Solution {
public:
    TreeNode* str2tree(string s) {
        if(s.empty()) return NULL;
        int i = 0, n = s.size();
        // read first num
        while(i < n && s[i] != '(' && s[i] != ')')
            i++;
        TreeNode *root = new TreeNode(stoi(s.substr(0, i)));
        //use stack to track parent node
        stack<TreeNode*> st;
        st.push(root); 
        
        while(i < n){
            if(s[i] == '('){
                i++;
                //read num
                string str = "";
                while(s[i] != '(' && s[i] != ')' && i < n){
                    str += s[i]; i++;
                }
                //create node
                TreeNode* temp = new TreeNode(stoi(str));
                //use st to find left or right
                TreeNode* parent =st.top();
                if(parent->left == NULL) parent->left = temp; 
                else parent->right = temp;
                
                st.push(temp);
            }
            if(s[i] == ')'){
                st.pop();
                i++;
            }
        }
        return root;
    }
};
```