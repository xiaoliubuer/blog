---
layout: post
title:  "244. Shortest Word Distance II"
date: 2019-05-27 18:32:00 -0400
categories: articles
---
Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list. Your method will be called repeatedly many times with different parameters. 

Example:
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].
```
Input: word1 = “coding”, word2 = “practice”
Output: 3
```
```
Input: word1 = "makes", word2 = "coding"
Output: 1
```
Note:
You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

```c++
class WordDistance {
public:
    WordDistance(vector<string>& words) {
        for ( int i = 0; i < words.size(); i++ ) {
            mapping[words[i]].push_back(i);
        }
    }
    
    int shortest(string word1, string word2) {
        vector<int> lists1 = mapping[word1];
        vector<int> lists2 = mapping[word2];
        int idx1 = 0, idx2 = 0;
        int res = INT_MAX;
        while ( idx1 < lists1.size() && idx2 < lists2.size() ) {
            res = min(res, abs(lists1[idx1] - lists2[idx2]));
            if ( lists1[idx1] < lists2[idx2]) idx1++;
            else if ( lists1[idx1] > lists2[idx2] ) idx2++;
        }
        return res;
    }
    
    unordered_map<string, vector<int>> mapping;
};

/**
 * Your WordDistance object will be instantiated and called as such:
 * WordDistance* obj = new WordDistance(words);
 * int param_1 = obj->shortest(word1,word2);
 */
```