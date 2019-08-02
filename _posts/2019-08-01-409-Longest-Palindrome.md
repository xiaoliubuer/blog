---
layout: post
title:  "409. Longest Palindrome"
date: 2019-08-01 20:32:00 -0400
categories: articles
---
Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

Note:
Assume the length of given string will not exceed 1,010.

Example:
```
Input:
"abccccdd"

Output:
7
```
Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.


```c++
class Solution {
public:
    int longestPalindrome(string s) {
        vector<int> table(256);
        for(auto c: s)
            table[c]++;
        bool odd_flag = false;
        int ret = 0;
        for(int i=0; i<256; i++) {
            if(table[i] >= 2 && (table[i] & 1) == 0)
                ret += table[i];
            else if(table[i] & 1) {
                ret += table[i] - 1;
                if(!odd_flag)
                    odd_flag = true;
            }
        }
        return odd_flag ? ret + 1: ret;
    }
};
```
```c++
class Solution {
public:
    int longestPalindrome(string s) {
        unordered_map<char,int> freq;
        for (char c:s) {
            freq[c]++;
        }
        int result = 0, core = 0;
        for (auto p : freq) {
            result += p.second / 2 * 2;
            core += p.second & 1;
        }
        return result + min(core,1);
    }
};
```
```c++
class Solution {
public:
int longestPalindrome(string s) {
    map<char, int> mp;
    for(int i=0; i<s.size(); i++){
        mp[s[i]]++;
    }
    int length=0;
    int oddFirst=0; 
    for(map<char, int>::iterator it=mp.begin(); it!=mp.end(); it++){
        if(it->second%2==0){
            length=length+it->second;
        }
        else{
            if(oddFirst==0){
                length=length+it->second;
                oddFirst=1;
            }
            else{
                if(it->second>1){
                    length=length+it->second-1;
                }
            }
        }
    }
    return length;
}
};
```