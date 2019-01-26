---
layout: post
title:  "771. Jewels and Stones"
date: 2019-01-24 17:58:23 -0400
categories: articles
---
You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

Example 1:
```
Input: J = "aA", S = "aAAbbbb"
Output: 3
```
Example 2:
```
Input: J = "z", S = "ZZ"
Output: 0
```
Note:
```
S and J will consist of letters and have length at most 50.
The characters in J are distinct.
```
# Function signature
```c++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        
    }
};
```
# 题意
有多少个在J里面的char出现在了S里。
# 想法
set？？太大啦吧？
目前想到的就是set。
# 参考答案
```c++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
    int res = 0;
    if ( J.size() == 0 || S.size() == 0 ) return res;
    for (int i = 0; i < S.size(); ++i){
    	for (int j = 0; j < J.size(); ++j){
    		if (S[i] == J[j]) res++;
    	}
      }
      return res;
    }
};
```