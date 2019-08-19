---
layout: post
title:  "680. Valid Palindrome II"
date: 2019-08-18 17:30:00 -0400
categories: articles
---
Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

Example 1:
```
Input: "aba"
Output: True
```
Example 2:
```
Input: "abca"
Output: True
```

Explanation: You could delete the character 'c'.
Note:
```
The string will only contain lowercase characters a-z. The maximum length of the string is 50000.
```
```c++
class Solution {
public:
    bool validPalindrome(string s) {
        bool deleted = false;
        int l = 0, r = s.size() - 1;
        while (l < r) {
            if (s[l] != s[r]) {
                if (is_valid(s, l, r - 1)) return true;
                if (is_valid(s, l + 1, r)) return true;
                return false;
            }
            l++; r--;
        }
        return true;
    }
private:
    bool is_valid(string s, int l, int r) {
        while (l < r) {
            if (s[l++] != s[r--]) return false;
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    bool validPalindrome(string s) {
        int i, j, i0, j0;
        for(i = 0, j = s.size() - 1; i < j && s[i] == s[j]; ++i, --j);
        if(i < j) {
            for(i0 = i + 1, j0 = j; i0 < j0 && s[i0] == s[j0]; ++i0, --j0);
            if(i0 >= j0) return true;
            for(i0 = i, j0 = j - 1; i0 < j0 && s[i0] == s[j0]; ++i0, --j0);
            return i0 >= j0;
        } return true;
    }
};
```
```c++
class Solution {
public:
    bool validPalindrome(string s) {
        std::ios_base::sync_with_stdio(false);
        
        if (s.size() < 3)
            return true;
        
        int left = 0;
        int right = s.size() - 1;
        while (s[left] == s[right] and left <= right) {
            left++; 
            right--;
        }
        
        if ( left >= right or
               isPalindrome(s, left + 1, right) or
                 isPalindrome(s, left, right - 1) )
            return true;
        
        return false;
    }
    
    bool isPalindrome(string& s, int left, int right) {
        while (left <= right) {
            if (s[left] != s[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
};
```

```c++
class Solution {
    bool helper(string const & s, int l, int r, bool removed)
    {
        while (l < r)
        {
            if (s[l] != s[r])
            {
                if (removed) return false;
                return helper(s, l+1, r, true) || helper(s, l, r-1, true);
            }
            ++l;
            --r;
        }
        return true;
    }
public:
    bool validPalindrome(string s) {
        return helper(s, 0, s.size()-1, false);
    }
};
```
```c++
class Solution {
public:
    bool validPalindrome(string s) {
        int slen = s.size();
        if(slen==1) return false;
        if(slen==2) return true;
        for(int ptr1=0,ptr2=slen-1;ptr1<ptr2;ptr1++,ptr2--){
            if(s[ptr1]!=s[ptr2]){
                return isP(s,ptr1,ptr2-1) || isP(s,ptr1+1,ptr2);
            }
        }
        return true;
    }
    bool isP(string& s,int is, int ie){
        for(;is<ie;is++,ie--){
            if(s[is]!=s[ie]) return false;
        }
        return true;
    }
};
```