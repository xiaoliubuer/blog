---
layout: post
title:  ". 472. Concatenated Words"
date: 2019-01-20 19:54:23 -0400
categories: articles
---
Given a list of words (without duplicates), please write a program that returns all concatenated words in the given list of words.
A concatenated word is defined as a string that is comprised entirely of at least two shorter words in the given array.

Example:
```
Input: ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]

Output: ["catsdogcats","dogcatsdog","ratcatdogcat"]

Explanation: "catsdogcats" can be concatenated by "cats", "dog" and "cats"; 
 "dogcatsdog" can be concatenated by "dog", "cats" and "dog"; 
"ratcatdogcat" can be concatenated by "rat", "cat", "dog" and "cat".
```
Note:
The number of elements of the given array will not exceed 10,000
The length sum of elements in the given array will not exceed 600,000.
All the input string will only include lower case letters.
The returned elements order does not matter.
# Function signature
```c++
class Solution {
public:
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
        
    }
};
```
# 题意
就是给一个list的词，找到所有有联系的词。这种词的特点就是有其他的词可以组成。

# 想法
遍历每个词，用当前的和其余的进行对比。找到是不是存在于其余的，如果存在。就把其余的进行subset，这个不行。因为这样的话有可能一个但是存在于新拼凑起来的这个词里面。但是其实不满足条件。

还能怎么做？？
vector？ set？现排序？？按照长度排序？然后
就用个set把所有的单词都存起来。
然后遍历每个点词，对于每个点词都进行DFS的匹配，看看能不能找到最终匹配完成的
# 尝试解解
```c++
// Accepted ! Surprise!!! 
class Solution {
public:
	bool helper(string word, unordered_set<string>& checkBuf, int idx){
		if ( word.size() == 0) return false; // Be careful
        if (idx == word.size()) return true;
		for (int i = 1; i <= word.size() - idx; ++i){
			string sub = word.substr(idx, i);
			if ( checkBuf.find(sub) != checkBuf.end() && i < word.size() && helper(word, checkBuf, idx + i)){
                return true;
			}
		}
		return false;
	}
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) {
    	vector<string> res;
    	unordered_set<string> checkBuf;
    	if ( words.size() == 0 ) return res;
    	for (string i : words){
    		checkBuf.insert(i);
    	}

    	for (int i = 0; i < words.size(); ++i){
    		if ( helper(words[i], checkBuf, 0) ){
    			res.push_back(words[i]);
    		}
    	}
    	return res;
    }
};
```