---
layout: post
title:  "159. Longest Substring with At Most Two Distinct Characters"
date: 2019-07-27 11:25:00 -0400
categories: articles
---
Given a string s , find the length of the longest substring t  that contains at most 2 distinct characters.
Example 1:
```
Input: "eceba"
Output: 3
Explanation: t is "ece" which its length is 3.
```
Example 2:
```
Input: "ccaabbb"
Output: 5
Explanation: t is "aabbb" which its length is 5.
```
```c++
class Solution {
public:
    int lengthOfLongestSubstringTwoDistinct(string s) {
        if ( s.length() == 0 ) return 0;
        
        int slow = 0;
        int i = 0;
        int mark[256] = {0};
        int distinct_num = 0;
        int res = 0;
        
        while ( i < s.length() ) {
            if ( distinct_num < 2 || mark[s[i]] > 0){
                res = max(res, i - slow + 1);
                if ( mark[s[i]] == 0)
                    distinct_num++;
                mark[s[i]]++;
                i++;
            }
            else{
                mark[s[slow]]--;
                if ( mark[s[slow]] == 0 ) 
                    distinct_num--;
                slow++;
            }
        }
        return res;
    }
};
```