---
layout: post
title:  "245. Shortest Word Distance III"
date: 2019-07-28 15:53:00 -0400
categories: articles
---

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

word1 and word2 may be the same and they represent two individual words in the list.

Example:
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].
```
Input: word1 = “makes”, word2 = “coding”
Output: 1
Input: word1 = "makes", word2 = "makes"
Output: 3
```
Note:
You may assume word1 and word2 are both in the list.
```c++
class Solution {
public:
int shortestWordDistance(vector<string>& words, string word1, string word2) {
    long long dist = INT_MAX, i1 = dist, i2 = -dist;
    bool same = word1 == word2;
    for (int i=0; i<words.size(); i++) {
        if (words[i] == word1) {
            i1 = i;
            if (same)
                swap(i1, i2);
        } else if (words[i] == word2) {
            i2 = i;
        }
        dist = min(dist, abs(i1 - i2));
    }
    return dist;
}
};
```

```c++
class Solution {
public:
int shortestWordDistance(vector<string>& words, string word1, string word2) {
    long long dist = INT_MAX, i1 = dist, i2 = -dist;
    for (int i=0; i<words.size(); i++) {
        if (words[i] == word1)
            i1 = i;
        if (words[i] == word2) {
            if (word1 == word2)
                i1 = i2;
            i2 = i;
        }
        dist = min(dist, abs(i1 - i2));
    }
    return dist;
}
};
```