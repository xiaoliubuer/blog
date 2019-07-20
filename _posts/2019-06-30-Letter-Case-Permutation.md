---
layout: post
title:  "784. Letter Case Permutation"
date: 2019-06-30 19:06:00 -0400
categories: articles
---

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

Examples:
```
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]
```
```
Input: S = "3z4"
Output: ["3z4", "3Z4"]
```
```
Input: S = "12345"
Output: ["12345"]
```
Note:

S will be a string with length between 1 and 12.
S will consist only of letters or digits.

```c++
class Solution {
public:
    vector<string> letterCasePermutation(string S) {
        vector<string> res;
        helper(0, S, "", res);
        return res;
    }
    void helper(int idx, string S, string tmp, vector<string>& res) {
        if ( idx == S.size() ){
            res.push_back(tmp);
            return;
        };
        if ( isalpha(S[idx]) ){
                char c = S[idx];
                tmp.push_back(tolower(c));
                helper(idx + 1, S, tmp, res);
                tmp.pop_back();
                tmp.push_back(toupper(c));
                helper(idx + 1, S, tmp, res);
                }
            else {
                tmp.push_back(S[idx]);
                helper(idx + 1, S, tmp, res);
            }
    }
};
```