---
layout: post
title:  "696. Count Binary Substrings"
date: 2019-08-18 18:17:00 -0400
categories: articles
---
Give a string s, count the number of non-empty (contiguous) substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

Example 1:
```
Input: "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".
```
Notice that some of these substrings repeat and are counted the number of times they occur.

Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.
Example 2:
```
Input: "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
```
Note:
```
s.length will be between 1 and 50,000.
s will only consist of "0" or "1" characters.
```
```c++
class Solution {
public:
    int countBinarySubstrings(string s) {
        if (s.length() <= 1) return 0;
        int res = 0, prev_num = 0, cur = 0, cur_num = 0;
        char prev_char;
        while (cur < s.length()) {
            if (s[cur] == prev_char || !cur) {
                ++cur_num;
            }
            else {
                res += min(prev_num, cur_num);
                prev_num = cur_num;
                cur_num = 1;
            }
            prev_char = s[cur];
            ++cur;
        }
        res += min(prev_num, cur_num);
        return res;
    }
};

```
```c++
class Solution {
public:
    int countBinarySubstrings(string s) {
        int cnt[2];
        int size = s.size();
        cnt[0] = cnt[1] = 0;
        
        char pre = s[0];
        cnt[pre - '0']++;
        int res = 0;
        int first = 1; // judge if it's the first group
        
        for (int i = 1; i < size; i++) {
            if (s[i] == pre) {
                cnt[pre - '0']++;
            } else {
                if (first == 1) {
                    first = 0;
                } else {
                    res += min(cnt[0], cnt[1]);
                }
                cnt[s[i] - '0'] = 1;
                pre = s[i];
            }
        }
        
        res += min(cnt[0], cnt[1]); // we need add again
        return res;
    }
};
```
```c++
class Solution {
public:
    int countBinarySubstrings(string s) {
        int num = 0;
        for(int i =0;i<s.size();) {
          int j = i;
          int n1 =0;
          int n2 =0;
          char cur = s[i];
          while(j<s.size()&&s[j]==cur) {
             n1++;
             j++;           
          }
          i = j;
          while(j<s.size()&&s[j]!=cur) {
              n2++;
              j++;
          }
          num += min(n1,n2);
        }
        return num;
    }
};
```