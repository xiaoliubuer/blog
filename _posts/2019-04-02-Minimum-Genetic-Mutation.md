---
layout: post
title:  "433. Minimum Genetic Mutation"
date: 2019-04-02 22:28:23 -0400
categories: articles
---
A gene string can be represented by an 8-character long string, with choices from "A", "C", "G", "T".

Suppose we need to investigate about a mutation (mutation from "start" to "end"), where ONE mutation is defined as ONE single character changed in the gene string.

For example, "AACCGGTT" -> "AACCGGTA" is 1 mutation.

Also, there is a given gene "bank", which records all the valid gene mutations. A gene must be in the bank to make it a valid gene string.

Now, given 3 things - start, end, bank, your task is to determine what is the minimum number of mutations needed to mutate from "start" to "end". If there is no such a mutation, return -1.

Note:

Starting point is assumed to be valid, so it might not be included in the bank.
If multiple mutations are needed, all mutations during in the sequence must be valid.
You may assume start and end string is not the same.
 

Example 1:
```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

return: 1
```
Example 2:
```
start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

return: 2
```
Example 3:
```
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

return: 3
```
```c++
// Accepted! 2019-07-15
class Solution {
public:
    int minMutation(string start, string end, vector<string>& bank) {
        unordered_set<string> record(bank.begin(), bank.end());
        queue<string> buff;
        buff.push(start);
        int res = 0;
        while( !buff.empty() ){
            res++;
            int buff_size = buff.size();
            for ( int i = 0; i < buff_size; i++ ) {
                string front = buff.front();
                buff.pop();
                for ( int j = 0; j < front.size(); j++ ) {
                    string temp = front;
                    for( char k = 'A'; k < 'Z'; k++ ) {
                        temp[j] = k;
                        if ( temp != front && record.count(temp) ) { 
                            if ( temp == end ) return res;
                            buff.push(temp);
                            record.erase(temp);
                        }
                    }
                }
            }
        }
        return -1;
    }
};
```

```c++
class Solution {
public:
    int minMutation(string start, string end, vector<string>& bank) {
        queue<string> toVisit;
        unordered_set<string> dict(bank.begin(), bank.end());
        int dist = 0;
        
        if(!dict.count(end)) return -1;
        
        toVisit.push(start);
        dict.insert(start); dict.insert(end);
        while(!toVisit.empty()) {
            int n = toVisit.size();
            for(int i=0; i<n; i++) {
                string str = toVisit.front();
                toVisit.pop();
                if(str==end) return dist;
                addWord(str, dict, toVisit);
            }
            dist++;
        }
        return -1;
        
    }
    
    void addWord(string word, unordered_set<string>& dict, queue<string>& toVisit) {
        dict.erase(word);
        for(int i=0; i<word.size(); i++) {
            char tmp = word[i];
            for(char c:string("ACGT")) {
                word[i] = c;
                if(dict.count(word)) {
                    toVisit.push(word);
                    dict.erase(word);
                }
            }
            word[i] = tmp;
        }
    }
};
```