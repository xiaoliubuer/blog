---
layout: post
title:  "320. Generalized Abbreviation"
date: 2019-07-18 21:48:00 -0400
categories: articles
---

Write a function to generate the generalized abbreviations of a word. 

Note: The order of the output does not matter.

Example:
```
Input: "word"
Output:
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```
```c++
class Solution {
public:
    vector<string> generateAbbreviations(string word) {
        vector<string> res;
        helper( res, word, "", 0 );
        return res;
    }
    void helper( vector<string>& res, string word, string cur, int idx ) {
        int num = word.size()-idx; // Interesting!!
        if( num < 1 ) {
            res.push_back( cur );
            return;
        }
        for( int i = 0; i <= num; i++ ) {
            string addS = ( i == 0 ) ? "" : to_string( i );
            if( cur.size()+i < word.size() ) {
                addS += word[idx+i];
                helper( res, word, cur+addS, idx+i+1 );
            } else {
                helper( res, word, cur+addS, idx+i );
            }
        }
    }
};
```