---
layout: post
title:  "127. Word Ladder"
date: 2019-04-02 22:29:23 -0400
categories: articles
---
Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time.
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.
Note:

Return 0 if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.
You may assume no duplicates in the word list.
You may assume beginWord and endWord are non-empty and are not the same.
Example 1:
```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```
Example 2:
```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```
```c++
// Accepted 2019-07-13
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> record(wordList.begin(), wordList.end());
        queue<string> buff;
        
        buff.push(beginWord);
        int res = 0;
        while(!buff.empty()){
            res++;
            int buff_size = buff.size();
            for ( int i = 0; i < buff_size; i++ ) {
                string front = buff.front();
                buff.pop();
                for ( int j = 0; j < front.size(); j++ ) {
                    string temp = front;
                    for (char k = 'a'; k <='z'; k++ ) {
                        temp[j] = k;
                        if ( front != temp && record.count(temp) ) {
                            if ( temp == endWord ) return res + 1;
                            buff.push(temp);
                            record.erase(temp);
                        }
                    }
                }
            }
        }
        return 0;
    }
};
```
```c++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        // Interesting part
        unordered_set<string> record(wordList.begin(), wordList.end());
        queue<string> buff;
        
        // BFS, push beginword in first, then each time push all 1 char different word in queue, to do the BFS.
        // 
        int res = 0;
        buff.push(beginWord);
        while (!buff.empty()) {
            res++;
            int curr_size = buff.size();
            for (int i = 0; i < curr_size; i++){
                string front = buff.front();
                buff.pop();
                
                // This for loop is the most tricky thing.
                for ( int k = 0; k < front.size(); k++) {
                    string temp = front;
                    for ( int c = 'a'; c <= 'z'; c++){
                        temp[k] = c;
                        if ( temp != front && record.count(temp)) {
                            if ( temp == endWord ) return res + 1;
                            buff.push(temp);
                            record.erase(temp);
                        }
                    }
                }
            }
        }
        return 0;
    }
};
```
```c++
// Accepted!! 2019-05-12
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> record(wordList.begin(), wordList.end());
        queue<string> buff;
        int res = 0;
        buff.push(beginWord);
        while (!buff.empty()) {
            res++;
            int curr_size = buff.size();
            for (int i = 0; i < curr_size; i++){
                string front = buff.front();
                buff.pop();
                for ( int k = 0; k < front.size(); k++) {
                    string temp = front;
                    for ( int c = 'a'; c <= 'z'; c++){
                        temp[k] = c;
                        if ( temp != front && record.count(temp)) {
                            if ( temp == endWord ) return res + 1;
                            buff.push(temp);
                            record.erase(temp);
                        }
                    }
                }
            }
        }
        return 0;
    }
};
```
```c++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end());
        
        queue<string> todo;
        todo.push(beginWord);
        int ladder = 1;
        while (!todo.empty()) {
            int n = todo.size();
            for (int i = 0; i < n; i++) {
                string word = todo.front();
                todo.pop();
                if (word == endWord) {
                    return ladder;
                }
                dict.erase(word);
                for (int j = 0; j < word.size(); j++) {
                    char c = word[j];
                    for (int k = 0; k < 26; k++) {
                        word[j] = 'a' + k;
                        if (dict.find(word) != dict.end()) {
                            todo.push(word);
                        }
                     }
                    word[j] = c;
                }
            }
            ladder++;
        }
        return 0;
    }
};
```
```c++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end());
        queue<string> myqueue;
        int ladder = 1;
        myqueue.push(beginWord);
        
        while(!myqueue.empty()){
        	int n = myqueue.size();
            for (int i = 0 ; i < n; i++) {
                string front = myqueue.front();
                myqueue.pop();
                if (front == endWord) 
                    return ladder;
                dict.erase(front);
                for (int j = 0; j < front.size(); j++){
                    char c = front[j];
                    for (int k = 0; k < 26; k++ ){
                        front[j] = 'a' + k;
                        if (dict.find(front) != dict.end()){
                            myqueue.push(front);
                        }
                    }
                    front[j] = c;
                }
            }
        ladder++;
        }
        return 0;
    }
};
```
```c++
// Time out: Logic should be right
class Solution {
public:
    int res = 999;
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_map<string, bool> visited;
        for ( auto i : wordList ) {
            visited[i] = false;
        }
        helper(beginWord, endWord, visited, wordList, 1);
        return res == 999 ? 0 : res;
    }
    
    void helper(string b, string e, unordered_map<string, bool>& visited, vector<string> wordList, int idx){
        if ( b == e ) {
            res = min(res, idx);
            return;
        }
        for ( int i = 0; i < wordList.size(); i++ ) {
            string w = wordList[i];
            if ( !visited[w] && valid(b, w) ){
                visited[w] = true;
                helper(w, e, visited, wordList, idx+1);
                visited[w] = false;
            }
        }
    };
    bool valid(string a, string b){
        if ( a.size() != b.size() ) return false;
        int count = 0;
        for ( int i = 0; i < a.size(); i++ ){
            if (a[i] != b[i]) count++;
        }
        return count< 2;
    }
};
```