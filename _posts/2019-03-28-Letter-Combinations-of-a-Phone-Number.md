---
layout: post
title:  "17. Letter Combinations of a Phone Number"
date: 2019-03-28 20:54:23 -0400
categories: articles
---
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.



Example:
```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> keys = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> res;
        if ( digits.size() == 0 ) return res;
        res.push_back("");
        for (auto i : digits) {
            int number = i - '0';
            vector<string> temp;
            for ( int j = 0 ; j < keys[number].size(); j++ ){
                for ( int k = 0; k < res.size(); k++ ) {
                    string cur = res[k];
                    cur.push_back(keys[number][j]);
                    temp.push_back(cur);
                }
            }
            res = temp;
        }
        return res;
    }
};
```
```c++
// 2019-04-23
// Accepted!!
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        string keys[10] = {" ", " ", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
        vector<string> res;
        if ( digits.size() == 0 ) return res;
        res.push_back("");
        for (auto i : digits) {
            int num = i - '0';
            string key = keys[num];
            vector<string> temp;
            for (auto j : key){
                for (int k = 0; k < res.size(); k++ ){
                    temp.push_back( res[k] + j);
                }
            }
            res = temp;
        }
        return res;
    }
};
```

```c++
// 2019-04-23
// Accepted!!! DFS
class Solution {
public:
    void helper(string digits, int idx, string curr, vector<string>& keys, vector<string>& res) {
        if (idx >= digits.size()){
            res.push_back(curr);
            return;
        };
        string letters = keys[digits[idx] - '0']; // 操，注意输入的是"23"
        for ( auto i : letters ) {
            string temp = curr + i;
            helper(digits, idx + 1, temp, keys, res);
        }
}
    vector<string> letterCombinations(string digits) {
        vector<string> keys = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
        vector<string> res;
        if ( digits.size() == 0 ) return res;
        helper(digits, 0, "", keys, res);
        return res;
    }
};
```