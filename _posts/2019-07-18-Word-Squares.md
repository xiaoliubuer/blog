---
layout: post
title:  "425. Word Squares"
date: 2019-07-18 21:51:00 -0400
categories: articles
---

Given a set of words (without duplicates), find all word squares you can build from them.

A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

For example, the word sequence ["ball","area","lead","lady"] forms a word square because each word reads the same both horizontally and vertically.

b a l l
a r e a
l e a d
l a d y
Note:
There are at least 1 and at most 1000 words.
All words will have the exact same length.
Word length is at least 1 and at most 5.
Each word contains only lowercase English alphabet a-z.
Example 1:
```
Input:
["area","lead","wall","lady","ball"]

Output:
[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```
Example 2:
```
Input:
["abat","baba","atan","atal"]

Output:
[
  [ "baba",
    "abat",
    "baba",
    "atan"
  ],
  [ "baba",
    "abat",
    "baba",
    "atal"
  ]
]
```
Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).

```c++
class Solution {
public:
    
    int n, len;
    vector<vector<string>> ans;
    unordered_map<string, vector<string>> prefixWords;
    
    void dfs(vector<string> &res) {
        if (res.size() >= len) {
            ans.push_back(res);
            return ;
        }
        int letterPos = res.size();
        string prefix ="";
        for (int i=0; i<res.size(); i++)
            prefix.push_back(res[i][letterPos]);
        vector<string>& candidates = prefixWords[prefix];
        for (int i=0; i<candidates.size(); i++) {
            res.push_back(candidates[i]);
            dfs(res);
            res.pop_back();
        }
    }
    
    vector<vector<string>> wordSquares(vector<string>& words) {
        n = words.size();
        len = words[0].length();
        prefixWords.clear();
        for (int i=0; i<n; i++) {
            string &str = words[i];
            for (int j=0; j<=len; j++)
                prefixWords[str.substr(0, j)].push_back(str);
        }
        ans.clear();
        vector<string> res;
        dfs(res);
        return ans;
    }
};
```