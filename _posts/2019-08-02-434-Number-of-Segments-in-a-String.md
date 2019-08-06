---
layout: post
title:  "434. Number of Segments in a String"
date: 2019-08-02 20:04:00 -0400
categories: articles
---
Count the number of segments in a string, where a segment is defined to be a contiguous sequence of non-space characters.

Please note that the string does not contain any non-printable characters.

Example:
```
Input: "Hello, my name is John"
Output: 5
```
```c++
class Solution {
public:
    int countSegments(string s) {
        int ans = 0, j = 0, i = s.size() - 1;
        
        // trim end spaces
        while(j < s.size() && s[j] == ' ')j++;
        while(i >= 0 && s[i] == ' ')i--;
        
        while(j <= i) {
            while(j <= i && s[j] != ' ')j++; // one word complete
            ans++;
            while(j <= i && s[j] == ' ')j++; // after a word, forward till start of next word
        }
        return ans;   
    }
};
```