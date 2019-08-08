---
layout: post
title:  "520. Detect Capital"
date: 2019-08-07 20:22:00 -0400
categories: articles
---
Given a word, you need to judge whether the usage of capitals in it is right or not.

We define the usage of capitals in a word to be right when one of the following cases holds:

All letters in this word are capitals, like "USA".
All letters in this word are not capitals, like "leetcode".
Only the first letter in this word is capital, like "Google".
Otherwise, we define that this word doesn't use capitals in a right way.

Example 1:
```
Input: "USA"
Output: True
```
Example 2:
```
Input: "FlaG"
Output: False
```
Note: The input will be a non-empty word consisting of uppercase and lowercase latin letters.

```c++
class Solution {
public:
    bool detectCapitalUse(string word) {
        int count=0 ;
        for(size_t i=0;i<word.size();i++){
            if(word[i]==toupper(word[i])){
                count++;
            }
        }

        if (count==0||count==word.size()){
            return true;
        }
        if(count==1 && word[0]==toupper(word[0])){
            return true;
        }
        else{
            return false;
        }
    }
};
```
```c++
class Solution {
public:
bool isUpper(char c) {
    if (c >= 'A' && c <= 'Z') {
        return true;
    }
    return false;
}
bool detectCapitalUse(string word) {
    bool rv = true;
    if (isUpper(word[word.size() - 1])) {
        for (int i = 0; i < word.size() - 1; ++i) {
            rv &= isUpper(word[i]);
        }
    } else {
        for (int i = 1; i < word.size() - 1; ++i) {
            rv &= !isUpper(word[i]);
        }
    }
    return rv;
}
};
```
```c++
class Solution {
public:
    bool detectCapitalUse(string word) {
        int q[100],w=0;
        bool e=0;
        for(size_t i=0;i<word.size();i++){
            if(word[i]<91){
                q[i]=0;
            }
            else{
                q[i]=1;
            }
        }
        for(size_t i=0;i<word.size();i++){
            if(q[i]<1){
                w++;
            }
            if(q[0]==1){
                if(q[i]==0&&i>0){
                    w=-1;
                    break;
                }
            }
        }
        if(w==0||w==word.size()||w==1){
            e=1;
        }
        return e;
    }
};
```