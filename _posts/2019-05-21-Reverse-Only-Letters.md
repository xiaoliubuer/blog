---
layout: post
title:  "917. Reverse Only Letters"
date: 2019-05-21 22:41:00 -0400
categories: articles
---
Given a string S, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.
Example 1:
```
Input: "ab-cd"
Output: "dc-ba"
```
Example 2:
```
Input: "a-bC-dEf-ghIj"
Output: "j-Ih-gfE-dCba"
```
Example 3:
```
Input: "Test1ng-Leet=code-Q!"
Output: "Qedo1ct-eeLg=ntse-T!"
```

Note:

S.length <= 100
33 <= S[i].ASCIIcode <= 122 
S doesn't contain \ or "

```c++
class Solution {
public:
    string reverseOnlyLetters(string S) {
        for (int i = 0, j = S.length() - 1; i < j; ++i, --j) {
            while (i < j && !isalpha(S[i])) ++i;
            while (i < j && !isalpha(S[j])) --j;
            swap(S[i], S[j]);
        }
        return S;
    }
};
```