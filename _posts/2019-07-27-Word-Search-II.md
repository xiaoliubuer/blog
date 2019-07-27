---
layout: post
title:  "212. Word Search II"
date: 2019-07-27 18:46:00 -0400
categories: articles
---
Given a 2D board and a list of words from the dictionary, find all words in the board.
Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

Example:
```
Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```
Note:
```
All inputs are consist of lowercase letters a-z.
The values of words are distinct.
```

```c++
class Solution {
public:
    struct Node {
        string word;
        Node* child[26];
        Node() { memset(child, 0, sizeof(child)); }
    };
    
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        Node trieRoot, *cur;
        
		// Build root trie
        for (auto& word : words) {
            cur = &trieRoot;
            for (auto c : word)
                cur = cur->child[c-'a'] ? cur->child[c-'a'] : cur->child[c-'a'] = new Node();
            cur->word = word;
        }
        
		// Find words using every col/row as a starting point
        vector<string> results;
        for (int row = 0; row < board.size(); ++row)
            for (int col = 0; col < board[0].size(); ++col)
                dfs(row, col, &trieRoot, results, board);
        return results;
    }
    
    void dfs(int row, int col, Node *cur, vector<string>& results, vector<vector<char>>& board) {
        char c = board[row][col];
        Node* next;
        
        if (c == '#' || !(next = cur->child[c-'a']))
            return;
        if (next->word.length()) {
            results.push_back(next->word);
            next->word = "";
        }

        board[row][col] = '#'; // mark char in use
        if (row+1 < board.size()   ) dfs(row+1, col, next, results, board);
        if (col+1 < board[0].size()) dfs(row, col+1, next, results, board);
        if (row-1 >= 0             ) dfs(row-1, col, next, results, board);
        if (col-1 >= 0             ) dfs(row, col-1, next, results, board);
        board[row][col] = c; // no longer in use, reset
    }
};
```