---
layout: post
title:  "557. Reverse Words in a String III"
date: 2019-08-11 16:34:00 -0400
categories: articles
---

Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

Example 1:
```
Input: "Let's take LeetCode contest"
Output: "s'teL ekat edoCteeL tsetnoc"
```
Note:
```
In the string, each word is separated by single space and there will not be any extra space in the string.
```
```c++
class Solution {
public:
    
    string reverseWords(string s) {
        int i = 0, j = 0, k;
        while(j < s.size()) {
            i = j;
            while(s[j] != ' ' && j < s.size()) j++;
            k = j-1;
            while(i < k) swap(s[i++], s[k--]);
            j++;
        }
        return s;
    }
};
```
```c++
class Solution {
public:
    string reverseWords(string s) {
        int i=0,indx=0,j=0;
        while(i<s.size()){
            indx=i;
            while(s[i]!=' ' && i<s.size()) i++;
            j=i-1;
            while(indx<j) swap(s[indx++],s[j--]);
            i++;
        }
        return s;
    }
};
```

```c++
class Solution {
public:
    string reverseWords(string s) {
        int start=0,end=0,l=s.size();
        while(start<l && end<l){
            while(end<l && s[end]!=' ') ++end;
            for(int i=start,j=end-1;i<j;++i,--j){
                swap(s[i],s[j]);
            }
            start=++end;
        }
        return s;
    }
};
```

```c++
class Solution {
public:
    string reverseWords(string s) {
        int j=0;
        for(int i=0;i<=s.size();++i){
            if(s[i]==' ' || i==s.size()){
                int p=i;
                for(;j<p;++j,--p){
                    swap(s[j],s[p-1]);
                }
                j=i+1;
            }
        }
        return s;
    }
};
```

```c++
class Solution {
public:
    string reverseWords(string s) {
        queue<char> q;
        int i = 0;
        for(;i<s.size(); i++) {
            if(s[i] != ' ') q.push(s[i]);
            else {
                int left = i-1;
                while(!q.empty()) {
                    s[left--] = q.front();
                    q.pop();
                }
            }
        }
		 int left = i-1;
		 while(!q.empty()) {
			s[left--] = q.front();
			q.pop();
		}
        return s;
    }
};
```