---
layout: post
title:  "** 76. Minimum Window Substring"
date: 2019-04-02 22:05:23 -0400
categories: articles
---
Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

Example:
```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```
Note:
```
If there is no such window in S that covers all characters in T, return the empty string "".
If there is such window, you are guaranteed that there will always be only one unique minimum window in S.
```
```c++
class Solution {
public:
    string minWindow(string s, string t) {
        if (s.size() == 0 || t.size() == 0) return "";
        vector<int> remaining(128, 0);
        int required = t.size();
        for (int i = 0; i < required; i++) 
        	remaining[t[i]]++;//做标记，标记着被检查的元素的个数个有多少个～

        // left is the start index of the min-length substring ever found
        int min = INT_MAX, start = 0, left = 0, i = 0;
        while(i <= s.size() && start < s.size()) {
            if(required) { //这个是用来判断左右的t的元素有没有都走完
                if (i == s.size()) break;
                remaining[s[i]]--;
                if (remaining[s[i]] >= 0) required--;
                i++;
            } else {
                if (i - start < min) { //这个之间的长度有没有小于当前最小
                    min = i -start;
                    left = start;
                }
                //这三句话可是重点啊！！！
                remaining[s[start]]++;
                if (remaining[s[start]] > 0) required++;
                start++;
            }
        }
        return min == INT_MAX? "" : s.substr(left, min);
    }
};
```