---
layout: post
title:  "471. Encode String with Shortest Length"
date: 2019-08-05 20:08:00 -0400
categories: articles
---
Given a non-empty string, encode the string such that its encoded length is the shortest.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times.

Note:

k will be a positive integer and encoded string will not be empty or have extra space.
You may assume that the input string contains only lowercase English letters. The string's length is at most 160.
If an encoding process does not make the string shorter, then do not encode it. If there are several solutions, return any of them is fine.
 

Example 1:
```
Input: "aaa"
Output: "aaa"
Explanation: There is no way to encode it such that it is shorter than the input string, so we do not encode it.
```
Example 2:
```
Input: "aaaaa"
Output: "5[a]"
Explanation: "5[a]" is shorter than "aaaaa" by 1 character.
```

Example 3:
```
Input: "aaaaaaaaaa"
Output: "10[a]"
Explanation: "a9[a]" or "9[a]a" are also valid solutions, both of them have the same length = 5, which is the same as "10[a]".
```

Example 4:
```
Input: "aabcaabcd"
Output: "2[aabc]d"
Explanation: "aabc" occurs twice, so one answer can be "2[aabc]d".
```

Example 5:
```
Input: "abbbabbbcabbbabbbc"
Output: "2[2[abbb]c]"
Explanation: "abbbabbbc" occurs twice, but "abbbabbbc" can also be encoded to "2[abbb]c", so one answer can be "2[2[abbb]c]".
```
```c++
class Solution {
    int numRepetition(string &s, string &t) {
        int cnt=0,i=0;
        while(i<s.length()) {
            if(s.substr(i,t.length())!=t) break;
            cnt++;
            i+=t.length();
        }
        return cnt;
    }
    string dfs(string s, unordered_map<string,string> &m) {
        if(s.length()<5) return s;
        if(m.count(s)) return m[s];
        string res = s;
        for(int i=0;i<s.length();i++) {
            string s1 = s.substr(0,i+1);
            int cnt = numRepetition(s,s1);
            string t;
            for(int k=1;k<=cnt;k++) {
                if(k==1) t=s1+dfs(s.substr(i+1),m);
                else t = to_string(k)+"["+dfs(s1,m)+"]"+dfs(s.substr(k*s1.length()),m);
                if(t.length()<res.length()) res=t;            
            }
        }
        m[s]=res;
        return res;        
    }
public:
    string encode(string s) {
        unordered_map<string,string> m;
        return dfs(s,m);
    }
};
```