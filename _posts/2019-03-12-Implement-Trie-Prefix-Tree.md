---
layout: post
title:  "208. Implement Trie (Prefix Tree)"
date: 2019-03-12 21:59:23 -0400
categories: articles
---
Implement a trie with insert, search, and startsWith methods.

Example:
```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```
Note:

You may assume that all inputs are consist of lowercase letters a-z.
All inputs are guaranteed to be non-empty strings.
```c++
// Accepted!!
class Trie {
public:
    /** Initialize your data structure here. */
    struct TreeNode {
        bool end = false;
        TreeNode* children[26] = {nullptr};
    };
    
    Trie() {
        root = new TreeNode();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        TreeNode* curr = root;
        for ( auto i : word ){
            int idx = i - 'a';
            if ( !curr->children[idx] ) {
                TreeNode* temp = new TreeNode();
                curr->children[idx] = temp;
            }
            curr = curr->children[idx];
        }
        curr->end = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        TreeNode* curr = root;
        for ( auto i : word ) {
            int idx = i - 'a';
            if ( curr->children[idx] ) curr = curr->children[idx];
            else return false;
        }
        return curr->end;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        TreeNode* curr = root;
        for ( auto i : prefix ){
            int idx = i - 'a';
            if (curr->children[idx]) curr = curr->children[idx];
            else return false;
        }
        return true;
    }
    TreeNode* root;
};
```