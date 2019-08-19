---
layout: post
title:  "647. Palindromic Substrings"
date: 2019-08-18 15:33:00 -0400
categories: articles
---
Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.
Example 1:
```
Input: "abc"
Output: 3
```
Explanation: Three palindromic strings: "a", "b", "c".
 

Example 2:
```
Input: "aaa"
Output: 6
```
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
 

Note:
```
The input string length won't exceed 1000.
```
```c++
class Solution {
public:
   int countSubstrings(string & S) 
   {
       int ans = 0;
       int n = int(S.size());
       for(int i = 0; i < n; i++)
       {
           // odd  length palindromes centered at 'i'
           int right = i + 1, left = i - 1;
           ans++; // A form string is a palindrome in itself
           while(left >= 0 && right < n && S[left] == S[right])
           {
               ans++;
               left--;
               right++;
           }
           // even length palindromes centered at 'i' and 'i - 1'
           left = i - 2, right = i + 1;
           if(i > 0 && S[i] == S[i - 1])
           { 
               ans++; // AA form string is a palindrome in itself
               while(left >= 0 && right < n && S[left] == S[right])
               {
                   ans++;
                   left--;
                   right++;
               }
           }
       }
       return ans;
   }
};
```