---
layout: post
title:  "* 737. Sentence Similarity II"
date: 2019-05-03 21:32:00 -0400
categories: articles
---
Given two sentences words1, words2 (each represented as an array of strings), and a list of similar word pairs pairs, determine if two sentences are similar.

For example, words1 = ["great", "acting", "skills"] and words2 = ["fine", "drama", "talent"] are similar, if the similar word pairs are pairs = [["great", "good"], ["fine", "good"], ["acting","drama"], ["skills","talent"]].

Note that the similarity relation is transitive. For example, if "great" and "good" are similar, and "fine" and "good" are similar, then "great" and "fine" are similar.

Similarity is also symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.

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
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
        
    }
};
```
# 题意
和734不一样的是，这个是 Note that the similarity relation is transitive.也就是相似性是可以传递的。如果A和B一样，B和C一样，A就和C一样。 那这道题该怎么做？
# 想法
在搜索的时候还有进行一个DFS，看一下能不能通过transitive找到。
# 尝试解解
```c++
// Wrong answer  63/117
class Solution {
public:
	bool transCompare(string word1, string word2, unordered_map<string, vector<string>>& buffer, unordered_set<string> visited){
		if (buffer.find(word1) == buffer.end()) return false;
		if (visited.find(word1) != visited.end()) return false;
		if (find(buffer[word1].begin(), buffer[word1].end(), word2) != buffer[word1].end()) 
			return true;
		else{
			vector<string> elm = buffer[word1];
			for (int i = 0; i < elm.size(); ++i){
				visited.emplace(elm[i]);
				if (transCompare(elm[i], word2, buffer, visited)) return true;
			}
		}
		return false;
	}

    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
    	unordered_map<string, vector<string>> buffer;
    	if (words1.size() != words2.size()) return false;

    	for ( int i = 0; i < pairs.size(); i++ ) {
    		if (buffer.find(pairs[i].first) == buffer.end()) {
    			buffer[pairs[i].first] = vector<string>{pairs[i].second};
    		}
    		else
    			buffer[pairs[i].first].push_back(pairs[i].second);
    	}
    	unordered_set<string> visited;

    	for (int i = 0; i < words1.size(); ++i){
    		if (words1[i] == words2[i] || transCompare(words1[i], words2[i], buffer) || transCompare(words2[i], words1[i], buffer), visited) return true;
    	}
    	return false;
    }
};
```
# Shame answer
```c++
// Union Find!!!!!!! Shame answer!!!
class Solution { // Union Find
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<vector<string>>& pairs) {
        if ( words1.size() != words2.size() ) return false;
        unordered_map<string, string> mapping;
        for ( auto i : pairs ) {
            string p1 = find(mapping, i[0]);
            string p2 = find(mapping, i[1]);
            if ( p1 != p2 ) mapping[p1] = p2;
        }
        
        for ( int i = 0; i < words1.size(); i++ ){
            string p1 = find(mapping, words1[i]);
            string p2 = find(mapping, words2[i]);
            if ( words1[i] != words2[i] && p1 != p2 ) return false;
        }
        return true;
    }
    
    string find(unordered_map<string, string>& mapping, string s) {
        if ( mapping.count(s) == 0 ){ // This piece code must be here first to verify the s exist here or not.
            mapping[s] = s;
            return mapping[s];
        }
        else{
            if ( mapping[s] == s) return mapping[s];
            else{
                string p = find(mapping, mapping[s]);
                mapping[s] = p;
                return mapping[s];
            }
        }
    }
};
```
```c++
// Accepted! a little bit cleaner
class Solution { // Union Find
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<vector<string>>& pairs) {
        if ( words1.size() != words2.size() ) return false;
        unordered_map<string, string> mapping;
        
        for ( auto i : pairs ) {
            string p1 = find(mapping, i[0]);
            string p2 = find(mapping, i[1]);
            if ( p1 != p2 ) mapping[p1] = p2;
        }
        
        for ( int i = 0; i < words1.size(); i++ ){
            string p1 = find(mapping, words1[i]);
            string p2 = find(mapping, words2[i]);
            if ( words1[i] != words2[i] && p1 != p2 ) return false;
        }
        return true;
    }
    
    string find(unordered_map<string, string>& mapping, string s) {
        if ( mapping.count(s) == 0 ){
            mapping[s] = s;
        }
        else{
            if ( mapping[s] != s) {
                string p = find(mapping, mapping[s]);
                mapping[s] = p;
            }
        }
        return mapping[s];
    }
};
```
```c++
// DFS,   Shame answer!!!
class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<vector<string>>& pairs) {
        if ( words1.size() != words2.size() ) return false;
        unordered_map<string, unordered_set<string>> mapping;
        
        for ( auto i : pairs ) {
            mapping[i[0]].emplace(i[1]);
            mapping[i[1]].emplace(i[0]);
        }
        
        for ( int i = 0; i < words1.size(); i++ ) {
            unordered_set<string> visited;
            if ( !helper(mapping, words1[i], words2[i], visited)) return false; // if any of the word in sentence return false, then return false. Then return true
        }
        return true;
    }
    
    bool helper(unordered_map<string, unordered_set<string>>& mapping, string w1, string w2, unordered_set<string>& visited){
        if ( w1 == w2 ) return true;
        visited.emplace(w1);
        for ( auto i : mapping[w1]) {
            if ( visited.count(i) == 0 && helper(mapping, i, w2, visited)) // !visited.count(i)
                return true;
        }
        return false;
    }
};
```
```c++
// Iteration DFS!!
class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
        if (words1.size() != words2.size())
            return false;
        
        std::unordered_map<string, int> wordid;
        for (auto & x : pairs) {
            if (!wordid.count(x.first))
                wordid.emplace(x.first, wordid.size());
            if (!wordid.count(x.second))
                wordid.emplace(x.second, wordid.size());
        }
        
        vector<vector<int>> edges(wordid.size());
        for (auto & x : pairs) {
            int a = wordid[x.first];
            int b = wordid[x.second];
            edges[a].push_back(b);
            edges[b].push_back(a);
        }
        
        vector<int> colors(wordid.size(), -1);
        for (int i = 0; i < colors.size(); ++i) {
            if (colors[i] != -1)
                continue;
            
            colors[i] = i;
            
            vector<int> stack;
            stack.push_back(i);
            
            while (!stack.empty()) {
                int src = stack.back();
                stack.pop_back();
                
                for (int dest : edges[src]) {
                    if (colors[dest] == -1) {
                        colors[dest] = colors[src];
                        stack.push_back(dest);
                    }
                }
            }
        }
        
        for (int i = 0; i < words1.size(); ++i) {
            if (words1[i] == words2[i])
                continue;
            if (!wordid.count(words1[i]) || !wordid.count(words2[i]))
                return false;
            int id1 = wordid[words1[i]];
            int id2 = wordid[words2[i]];
            if (colors[id1] != colors[id2])
                return false;
        }
        
        return true;
    }
};
```