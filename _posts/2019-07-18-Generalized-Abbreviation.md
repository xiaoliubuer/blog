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
        vector<string> result;
        generateAbbreviationsHelper(word, "", 0, result, false);
        return result;
    }
    
    void generateAbbreviationsHelper(string& word, string abbr, int i, vector<string>& result, bool prevNum) {
        if (i == word.length()) {
            result.push_back(abbr);
            return;
        }
        
        generateAbbreviationsHelper(word, abbr+word[i], i+1, result, false);
        if (!prevNum) {
            // Add number abbreviations only when we added a character instead of an abbreviation earlier
            for (int len = 1; i+len <= word.length(); ++len) {
                generateAbbreviationsHelper(word, abbr+to_string(len), i+len, result, true);
            }
        }
    }
};
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