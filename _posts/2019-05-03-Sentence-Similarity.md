---
layout: post
title:  "734. Sentence Similarity"
date: 2019-05-03 20:32:00 -0400
categories: articles
---
Given two sentences words1, words2 (each represented as an array of strings), and a list of similar word pairs pairs, determine if two sentences are similar.

For example, "great acting skills" and "fine drama talent" are similar, if the similar word pairs are pairs = [["great", "fine"], ["acting","drama"], ["skills","talent"]].

Note that the similarity relation is not transitive. For example, if "great" and "fine" are similar, and "fine" and "good" are similar, "great" and "good" are not necessarily similar.

However, similarity is symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.

Also, a word is always similar with itself. For example, the sentences words1 = ["great"], words2 = ["great"], pairs = [] are similar, even though there are no specified similar word pairs.

Finally, sentences can only be similar if they have the same number of words. So a sentence like words1 = ["great"] can never be similar to words2 = ["doubleplus","good"].

Note:

The length of words1 and words2 will not exceed 1000.
The length of pairs will not exceed 2000.
The length of each pairs[i] will be 2.
The length of each words[i] and pairs[i][j] will be in the range [1, 20].
# Function signature
```c++
class Solution {
public:
    bool areSentencesSimilar(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
        
    }
};
```
# 题意
就是句子的相似性。给两个句子和一个相似性字典。如果句子1和句子2的每个字都对应词典，那么这个两句子就是相似的。否则就不是。
# 想法
1. 长度不一样的话直接返回false。
2. 把字典存储一个map
3. 然后遍历两个句子。
如果 A[i] == B[i]|| map[A[i]] == B[i] || map[B[i]] == A[i], 那就过
# 尝试解解
```c++
class Solution {
public:
    bool areSentencesSimilar(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
        unordered_map<string, set<string>> dic;
        if ( words1.size() != words2.size() ) return false;
        if ( words1.size() == 0 && words2.size() == 0 ) return true;
        
        for (auto i : pairs){
            dic[i.first].emplace(i.second);
        }
        
        for ( int i = 0 ; i < words1.size(); i++ ) {
            if ( words1[i] != words2[i] && dic[words1[i]].count(words2[i]) != 1 && dic[words2[i]].count(words1[i]) !=1 ) 
                return false;
        }
        return true;
    }
};
```
```c++
// Accepted! Clean answer
// 2019-05-03
class Solution {
public:
    bool areSentencesSimilar(vector<string>& words1, vector<string>& words2, vector<vector<string>>& pairs) {
        if ( words1.size() != words2.size() ) return false;
        unordered_map<string, unordered_set<string>> mapping;
        for ( auto i : pairs) { // Build mapping
            string w1 = i[0];
            string w2 = i[1];
            mapping[w1].emplace(w2);
            mapping[w2].emplace(w1);
        }
        
        for ( int i = 0; i < words1.size(); i++ ) {
            string w1 = words1[i];
            string w2 = words2[i];
            if ( w1 != w2 && mapping[w1].count(w2) == 0 && mapping[w2].count(w1) == 0) return false;
        }
        return true;
    }
};
```
```
# 参考答案
```c++
class Solution {
public:
    bool areSentencesSimilar(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
		 if (words1.size() != words2.size()) return false;
         if (pairs.size() == 0 ) return words1 == words2;
		 unordered_map<string, unordered_set<string>> mymap;
		 for (int i = 0; i < pairs.size(); ++i){
		 	mymap[pairs[i].first].insert(pairs[i].second);
		 }

		 for (int i = 0; i < words1.size(); ++i){
		 	if (words1[i] != words2[i] && !mymap[words1[i]].count(words2[i]) && !mymap[words2[i]].count(words1[i]))
		 		return false;
		 }
		return true;
    }
};
```
