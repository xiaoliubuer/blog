---
layout: post
title:  "340. Longest Substring with At Most K Distinct Characters"
date: 2019-07-30 20:10:00 -0400
categories: articles
---
Given a string, find the length of the longest substring T that contains at most k distinct characters.

Example 1:
```
Input: s = "eceba", k = 2
Output: 3
Explanation: T is "ece" which its length is 3.
```
Example 2:
```
Input: s = "aa", k = 1
Output: 2
Explanation: T is "aa" which its length is 2.
```

```c++
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        if (s.length() == 0 || k == 0 ) return 0;
        
        int slow = 0;
        int i = 0;
        int mark[256] = {0};
        int distinct = 0;
        int res = 0;
        
        while ( i < s.length() ){
            //Condition to move fast pointer i
            if ( distinct < k || mark[s[i]] > 0 ) {
                res = max ( res, i - slow + 1); // Here is important
                if ( mark[s[i]] == 0 )
                    distinct++;
                mark[s[i]]++;
                i++;
            }
            else { // To move slow pointer slow
                mark[s[slow]]--;
                if ( mark[s[slow]] == 0 )
                    distinct--;
                slow++;
            }   
        }
        return res;
    }
};
```