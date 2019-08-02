---
layout: post
title:  "408. Valid Word Abbreviation"
date: 2019-08-01 20:29:00 -0400
categories: articles
---
Given a non-empty string s and an abbreviation abbr, return whether the string matches with the given abbreviation.

A string such as "word" contains only the following valid abbreviations:

["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
Notice that only the above abbreviations are valid abbreviations of the string "word". Any other string is not a valid abbreviation of "word".

Note:
Assume s contains only lowercase letters and abbr contains only lowercase letters and digits.

Example 1:
```
Given s = "internationalization", abbr = "i12iz4n":

Return true.
```
Example 2:
```
Given s = "apple", abbr = "a2e":

Return false.
```
```c++
class Solution {
public:
    bool validWordAbbreviation(string word, string abbr) {
        int i = 0, j = 0, num = 0;
        for(int i = 0; i < abbr.size(); i++) {
            if(isdigit(abbr[i])) {
                if(abbr[i] == '0' && num == 0) return false;        // check leading zero
                num = 10 * num + abbr[i] - '0';
            }
            else {
                j += num;
                num = 0;
                if(j >= word.size() || abbr[i] != word[j]) 
                    return false;
                j++;
            }
        }
        j += num;
        return j == word.size();
    }
};
```