---
layout: post
title:  "211. Add and Search Word - Data structure design"
date: 2019-05-27 13:18:00 -0400
categories: articles
---
Design a data structure that supports the following two operations:

void addWord(word)
bool search(word)
search(word) can search a literal word or a regular expression string containing only letters a-z or .. A . means it can represent any one letter.

Example:
```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```
Note:
You may assume that all words are consist of lowercase letters a-z.
```c++
class WordDictionary {
public:
    struct TreeNode {
        bool isEnd;
        TreeNode* children[26] = {nullptr};
    };
    /** Initialize your data structure here. */
    WordDictionary() : root(new TreeNode()) {
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
        TreeNode *curr = root;
        for (int i = 0; i < word.size(); i++) {
            char c = word[i];
            int idx = c - 'a';
            if ( !curr->children[idx])
                curr->children[idx] = new TreeNode();
            curr = curr->children[idx];
        }
        curr->isEnd = true;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool helper(TreeNode* root, string word, int idx){
        if ( !root ) return false;
        if ( idx == word.size() ) return root->isEnd; // Important!!!!
        char c = word[idx];
        if ( c != '.')
            return helper(root->children[c-'a'], word, idx+1);
        else {
            for ( int i = 0; i < 26; i++ ){
                if ( helper(root->children[i], word, idx + 1) ) return true;
            }
        }
        return false;
    }
    bool search(string word) {
        return helper(root, word, 0);
    }
    TreeNode* root;
};
```
```c++
class WordDictionary {
public:
    /** Initialize your data structure here. */
    struct TreeNode {
        int isEnd = false;
        TreeNode* children[26] = {nullptr};
    };
    
    WordDictionary() : root(new TreeNode()) {
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
        TreeNode* curr = root;
        for ( int i = 0; i < word.size(); i++ ) {
            char c = word[i];
            if ( curr->children[c-'a'] == nullptr){
                curr->children[c-'a'] = new TreeNode();
            }
            curr = curr-> children[c-'a'];
        }
        curr->isEnd = true;
    }

    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool helper(TreeNode* root, string word, int idx) {
        if (!root) return false;
        if (idx == word.size()) return root->isEnd; 
        char c = word[idx];
        if ( c != '.') {
            return helper(root->children[c-'a'], word, idx+1);
        }
        else {
            for ( int i = 0; i < 26; i++)
                if (helper(root->children[i], word, idx+1)) return true;
        }
        return false;
    }
    bool search(string word) {
        return helper(root, word, 0);
    }
    
    TreeNode* root;
};
```