---
layout: post
title:  "1087. Brace Expansion"
date: 2019-07-22 20:41:00 -0400
categories: articles
---

A string S represents a list of words.

Each letter in the word has 1 or more options.  If there is one option, the letter is represented as is.  If there is more than one option, then curly braces delimit the options.  For example, "{a,b,c}" represents options ["a", "b", "c"].

For example, "{a,b,c}d{e,f}" represents the list ["ade", "adf", "bde", "bdf", "cde", "cdf"].

Return all words that can be formed in this manner, in lexicographical order.

Example 1:
```
Input: "{a,b}c{d,e}f"
Output: ["acdf","acef","bcdf","bcef"]
```
Example 2:
```
Input: "abcd"
Output: ["abcd"]
```
Note:
```
1 <= S.length <= 50
There are no nested curly brackets.
All characters inside a pair of consecutive opening and ending curly brackets are different.
```
```c++
class Solution {
public:
    vector<string> expand(string S) {
        // Build the vector of options here
        vector<string> vec;
        for (int i=0;i<S.size();i++)
        {
            string cur = "";
            if (S[i] == '{'){
                i++;
                while (S[i] != '}')
                {
                    if (S[i] >= 'a' && S[i] <= 'z'){
                        cur.push_back(S[i]);
                    }
                    i++;
                }
                sort(cur.begin(), cur.end());
                vec.push_back(cur);
            } else if (S[i] >= 'a' && S[i] <= 'z')
            {
                cur.push_back(S[i]);
                vec.push_back(cur);
            }
        }
        
		// Searching started
        vector<string> rst;
        string cur = "";
        backTrack(0, rst, cur, vec);
        
        return rst;
    }
    
    void backTrack(int index, vector<string>& rst, string& cur, const vector<string>& v)
    {
        if (index >= v.size()){
            rst.push_back(cur);
            return;
        }
        
        for (const auto& c : v[index])
        {
            cur.push_back(c);
            backTrack(index+1, rst, cur, v);
            cur.pop_back(); // Backtrack
        }
    }
};
```