---
layout: post
title:  "451. Sort Characters By Frequency"
date: 2019-08-05 19:12:00 -0400
categories: articles
---
Given a string, sort it in decreasing order based on the frequency of characters.

Example 1:
```
Input:
"tree"

Output:
"eert"
```
Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
Example 2:
```
Input:
"cccaaa"

Output:
"cccaaa"
```
Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.
Example 3:
```
Input:
"Aabb"

Output:
"bbAa"
```
Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.


```c++
class Solution {
public:
    string frequencySort(string s) {
        map<char, int> freq;
        for (auto l:s)
            freq[l]++;
        
        vector<pair<char, int>> v {};
        
        for(auto x:freq)
            v.push_back(x);

        sort(v.begin(), v.end(),
            [](const pair<char,int> &l, const pair<char,int>&r){
               return l.second > r.second; 
            });
        
        string ret {""};
        
        for (auto &x: v)
            while(x.second--)
                ret += x.first;
        
        return ret;
    }
};
```
```c++
bool funct(pair<char,int>& a, pair<char,int>& b) {
    if(a.second > b.second) return true;
    return false;
}

class Solution {
public:
    string frequencySort(string s) {
        int data[256]={0};  //Create count array for each character
        for(int i=0; i<s.length(); i++) {
            data[s[i]]++;
        }
        vector<pair<char,int>> vec;
        for(int i=0; i<256; i++) {
            if(data[i] != 0) {   //take only those chars for which count was non zero
                vec.push_back(make_pair(i, data[i]));
            } 
        }
        sort(vec.begin(), vec.end(), funct);
        string str="";
        for(auto it: vec) {
            char c=it.first;
            for(int i=0; i<it.second; i++) 
                str.push_back(c);
        }
        return str;
    }
};
```
```c++

class Solution {
public:
    static bool compareInt(pair<int, char> &a, pair<int, char> &b) {
        return a.first > b.first;
    }
    
    string frequencySort(string s) {
        vector<pair<int, char>> freq(256);
        for(int i = 0; i < 256; i++) {
            freq[i] = {0, i};
        }
        
        for(int i = 0; i < s.length(); i++) {
            freq[s[i]].first++;
        }
        
        sort(freq.begin(), freq.end(), compareInt);
        
        string res = "";
        for(auto it : freq) {
            if(it.first == 0) {
                break;
            }
            string temp(it.first, it.second);
            res += temp;
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    
    string frequencySort(string s) {
        map<char, int> m;
        for (int i = 0; i < s.size(); ++i)
            m[s[i]]++;
        priority_queue<pair<int, char>> pq;
        for (auto i : m)
            pq.push(make_pair(i.second, i.first));
        string ans = "";
        while (!pq.empty()) {
            pair<int, char> tmp = pq.top();
            pq.pop();
            for (int i = 0; i < tmp.first; ++i)
                ans += tmp.second;
        }
        return ans;
    }
};
```
```c++
bool funct(pair<char,int> a, pair<char,int> b) {
    if(a.second > b.second) return true;
    if(a.second == b.second) return (a.first > b.first);
    return false;
}
class Solution {
public:
    string frequencySort(string s) {
        int data[256]={0};  //Create count array for each character
        for(int i=0; i<s.length(); i++) {
            data[s[i]]++;
        }
        vector<pair<char,int>> vec;
        for(int i=0; i<256; i++) {
            if(data[i] != 0) {   //take only those chars for which count was non zero
                vec.push_back(make_pair(i, data[i]));
            } 
        }
        sort(vec.begin(), vec.end(), funct);
        string str="";
        for(auto it: vec) {
            char c=it.first;
            for(int i=0; i<it.second; i++) 
                str.push_back(c);
        }
        return str;
    }
};
```