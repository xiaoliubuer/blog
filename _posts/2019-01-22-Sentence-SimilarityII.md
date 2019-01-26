---
layout: post
title:  "* 737. Sentence Similarity II"
date: 2019-01-22 20:53:23 -0400
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
```c++
// DFS 到底哪里可以减枝

class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
    	if ( words1.size() == words2.size() ) return false;

    }
};
```

```c++
// Union find. 如何用这种方法解答呢？？
// 这个就很有意思了～
// 
class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
        
    }
};
```
# Shame answer
```c++
class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& a, vector<string>& b, vector<pair<string, string>> pairs) {
        if (a.size() != b.size()) return false;
        map<string, string> m;
        for (pair<string, string> p : pairs) {
            string parent1 = find(m, p.first), parent2 = find(m, p.second);
            if (parent1 != parent2) m[parent1] = parent2;
        }

        for (int i = 0; i < a.size(); i++)
            if (a[i] != b[i] && find(m, a[i]) != find(m, b[i])) return false;

        return true;
    }

private:
    string find(map<string, string>& m, string s) {
        return !m.count(s) ? m[s] = s : (m[s] == s ? s : find(m, m[s]));
    }
};
```
```c++
class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& words1, vector<string>& words2, vector<pair<string, string>> pairs) {
        if (words1.size() != words2.size()) return false;
        unordered_map<string, unordered_set<string>> p;
        for (auto &vp : pairs) {
            p[vp.first].emplace(vp.second); // 双向都存
            p[vp.second].emplace(vp.first);
        }
        for (int i = 0; i < words1.size(); i++) {
            unordered_set<string> visited;
            if (isSimilar(words1[i], words2[i], p, visited)) continue;
            else return false;
        }
        return true;
    }
    
    bool isSimilar(string& s1, string& s2, unordered_map<string, unordered_set<string>>& p, unordered_set<string>& visited) {
        if (s1 == s2) return true;
        
        visited.emplace(s1);
        for (auto s : p[s1]) {
            if (!visited.count(s) && isSimilar(s, s2, p, visited))
                return true;
        }
        
        return false;
    }
};
```
```c++
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