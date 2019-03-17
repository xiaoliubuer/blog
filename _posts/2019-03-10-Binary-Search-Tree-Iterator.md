---
layout: post
title:  "173. Binary Search Tree Iterator"
date: 2019-03-10 20:14:23 -0400
categories: articles
---
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.
Calling next() will return the next smallest number in the BST.

Example:
```
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```
Note:
```
next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.
You may assume that next() call will always be valid, that is, there will be at least a next smallest number in the BST when next() is called.
```

#My Answer
```c++
//Accepted!!!
class BSTIterator {
public:
    
    void helper(TreeNode* root){
        if(!root) return;
        myqueue.push(root->val);
        helper(root->left);
        helper(root->right);
    }
    BSTIterator(TreeNode* root) {
        helper(root);
    }
    
    /** @return the next smallest number */
    int next() {
        int top = myqueue.top();
        myqueue.pop();
        return top;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !myqueue.empty();
    }
    priority_queue<int, vector<int>, greater<int>> myqueue;
};
```
```c++
//Accepted!! Using queue!
class BSTIterator {
public:
    void helper(TreeNode* root){
        if (!root) return;
        helper(root->left);
        myqueue.push(root->val);
        helper(root->right);
    }
    BSTIterator(TreeNode* root) {
        if (!root) return;
        helper(root);
    }
    
    /** @return the next smallest number */
    int next() {
        int front = myqueue.front();
        myqueue.pop();
        return front;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !myqueue.empty();
    }
    queue<int> myqueue;
};
```

```c++
class BSTIterator {
private:
    stack<TreeNode*> st;
public:
    BSTIterator(TreeNode *root) {
        find_left(root);
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        if (st.empty())
            return false;
        return true;
    }

    /** @return the next smallest number */
    int next() {
        TreeNode* top = st.top();
        st.pop();
        if (top->right != NULL)
            find_left(top->right);

        return top->val;
    }

    /** put all the left child() of root */
    void find_left(TreeNode* root)
    {
        TreeNode* p = root;
        while (p != NULL)
        {
            st.push(p);
            p = p->left;
        }
    }
};
```
