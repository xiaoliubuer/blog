---
layout: post
title:  ". 243. Shortest Word Distance"
date: 2019-01-29 21:34:23 -0400
categories: articles
---
Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

Example:
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Input: word1 = “coding”, word2 = “practice”
Output: 3
Input: word1 = "makes", word2 = "coding"
Output: 1
Note:
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.
# Function signature
```c++
class Solution {
public:
    int shortestDistance(vector<string>& words, string word1, string word2) {
        
    }
};
```
# 题意
就是算出两个词的最小距离
# 想法
直接做吧
# 尝试解解
```c++
// Accepted with watch answer, shame!!
class Solution {
public:
    int shortestDistance(vector<string>& words, string word1, string word2) {
    	if ( words.size() == 0 ) return 0;
    	int idx1 = -1, idx2 = -1, res = 999;
    	for (int i = 0; i < words.size(); ++i){
    		if ( words[i] == word1 ) idx1 = i;
    		else if ( words[i] == word2 ) idx2 = i;
    		if ( idx1 >= 0 && idx2 >= 0 ){
    			res = min( res, abs( idx1 - idx2 ));
    		}
    	}
        return res;
    }
};
```