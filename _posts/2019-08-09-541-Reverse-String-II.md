---
layout: post
title:  "541. Reverse String II"
date: 2019-08-09 20:50:00 -0400
categories: articles
---
Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original.
Example:
```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```
Restrictions:
```
The string consists of lower English letters only.
Length of the given string and k will in the range [1, 10000]
```
```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += 2*k){
            int left = i;
            int right = i + k - 1;
            int length = s.size();
            right = min(right, length - 1);
            while ( left < right ){
                swap(s[left++], s[right--]);
            }
        }
        return s;
    }
};
```
```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        int j=0;
        int n=s.size(), l, h;
        int K=2*k;
        while(j<n){
            l=j;
            h=min(n-1, l+k-1);
            while(l<h){
                swap(s[l], s[h]);
                l++;
                h--;
            }
            j+=K;
        }
        return s;
    }
};
```
```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        for(int i=0; i<s.length(); i+=2*k)
        {
           int left = i;
           int right = std::min(left + k - 1, (int)s.length() - 1);
		    while (left < right) {
			    swap(s[left++], s[right--]);
		    }                 
        }
        
        return s;
    }
};
```