---
layout: post
title:  "676. Implement Magic Dictionary"
date: 2019-08-18 16:59:00 -0400
categories: articles
---
Implement a magic directory with buildDict, and search methods.

For the method buildDict, you'll be given a list of non-repetitive words to build a dictionary.

For the method search, you'll be given a word, and judge whether if you modify exactly one character into another character in this word, the modified word is in the dictionary you just built.

Example 1:
```
Input: buildDict(["hello", "leetcode"]), Output: Null
Input: search("hello"), Output: False
Input: search("hhllo"), Output: True
Input: search("hell"), Output: False
Input: search("leetcoded"), Output: False
```
Note:
```
You may assume that all the inputs are consist of lowercase letters a-z.
For contest purpose, the test data is rather small by now. You could think about highly efficient algorithm after the contest.
Please remember to RESET your class variables declared in class MagicDictionary, as static/class variables are persisted across multiple test cases. Please see here for more details.
```
```c++
class MagicDictionary {
public:
    vector<vector<string>> table;
    /** Initialize your data structure here. */
    MagicDictionary() {
        
    }
    
    /** Build a dictionary through a list of words */
    void buildDict(vector<string> dict) {
        int max = 0;
        for (int i = 0; i < dict.size(); i++) {
            if (dict[i].size() > max)
                max = dict[i].size();
        }
        
        table.resize(max + 1);
        
        for (int i = 0; i < dict.size(); i++)
            table[dict[i].size()].push_back(dict[i]);
    }
    
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    bool search(string word) {
        if (table.size() <= word.size() || !table[word.size()].size())
            return false;
        
        for (int i = 0; i < table[word.size()].size(); i++) {
            string s = table[word.size()][i];
            
            int j = 0;
            bool ignore = true;
            for (j = 0; j < word.size(); j++) {
                if (word[j] != s[j]) {
                    if (ignore)
                        ignore = false;
                    else
                        break;
                }
            }
            
            if (j == word.size() && !ignore)
                return true;
        }
        
        return false;
    }
};
 ```
 ```c++
 class MagicDictionary {
    vector<string> data;
    
    bool same(string a, string b) {
        if (a.length() != b.length()) return false;
        
        int diff = 0;
        for (int i=0; i<a.length() && diff<=1; i++)
            if (a[i] != b[i])
                diff++;
        
        return diff == 1;
    }
    
public:
    /** Initialize your data structure here. */
    MagicDictionary() {
        data = vector<string>();
    }
    
    /** Build a dictionary through a list of words */
    void buildDict(vector<string> dict) {
        for (string s : dict) data.push_back(s);
    }
    
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    bool search(string word) {
        for (string s : data)
            if (same(s, word)) 
                return true;
        
        return false;
    }
};
```
```c++
class MagicDictionary {
public:
    /** Initialize your data structure here. */
    MagicDictionary() {
        
    }
    /** Build a dictionary through a list of words */
    void buildDict(vector<string> dict) {
        for(const auto& word : dict) {
            string t = word;
            for(int i = 0; i < t.size(); ++i) {
                char c = t[i];
                t[i] = '*';
                if(m.count(t)) {
                    m[t] = "";
                } else {
                    m[t] = word;
                }
                t[i] = c;
            }
        }
    }
    /** Returns if there is any word in the trie that equals to the given word after modifying exactly one character */
    bool search(string word) {
        string t = word;
        for(int i = 0; i < word.size(); ++i) {
            char c = t[i];
            t[i] = '*';
            if(m.count(t) &&(m[t].empty() || word != m[t])) {
                return true;
            }
            t[i] = c;
        }
        return false;
    }
private:
    unordered_map<string, string> m;
};
```