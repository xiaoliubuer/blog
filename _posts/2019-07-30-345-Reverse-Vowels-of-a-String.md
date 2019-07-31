---
layout: post
title:  "345. Reverse Vowels of a String"
date: 2019-07-30 20:26:00 -0400
categories: articles
---
Write a function that takes a string as input and reverse only the vowels of a string.

Example 1:
```
Input: "hello"
Output: "holle"
```
Example 2:
```
Input: "leetcode"
Output: "leotcede"
```
Note:
The vowels does not include the letter "y".

```c++
class Solution {
public:
    string reverseVowels(string s) {
    if (s.size() == 0) return s;
    unordered_set<char> vowel_set = {'a', 'e', 'i', 'o', 'u'}; 
    int left = 0, right = s.size() - 1;

    while( left < right ){
    	if (vowel_set.count(tolower(s[left])) == 0) {
            left++;
            continue;
        }
    	if (vowel_set.count(tolower(s[right])) == 0) {
            right--;
            continue;
        }
    	
        swap(s[left++], s[right--]);
    	}
    return s;
    }
};
```
```c++
class Solution {
public:
    string reverseVowels(string s) {
        unordered_set<char> vowels = {'a','e','i','o','u','A','E','I','O','U'};
        for(int i = 0, j = s.length() - 1; i < j; ++i, --j) {
            while(vowels.find(s[j]) == vowels.end() && i < j) {
                --j;
            }
            while(vowels.find(s[i]) == vowels.end() && i < j) {
                ++i;
            }
            if(i < j) {
                swap(s[i],s[j]);
            }
        }
        return s;
    }
};
```