---
layout: post
title:  "438. Find All Anagrams in a String"
date: 2019-08-02 20:13:00 -0400
categories: articles
---
Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

Example 1:
```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```
Example 2:
```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

```c++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> res;
        if ( s.size() == 0 || p.size() == 0 || s.size() < p.size()) return res;
        
        int mark[26] = {0};
        for ( auto i : p ) mark[i - 'a']++;
        int i = 0;
        int count = 0;
        int slow = 0;
        
        while ( i < s.size() ) {
            if ( mark[s[i] - 'a'] > 0 ){
                mark[s[i] - 'a']--;
                count++;
                if ( count == p.size() )
                    res.push_back(slow);
                i++;
            }
            else if ( slow < i ){
                mark[s[slow] - 'a']++;
                count--;
                slow++;
            }
            else{
                i++;
                slow++;
            }
            
        }
        return res;
    }
};
```