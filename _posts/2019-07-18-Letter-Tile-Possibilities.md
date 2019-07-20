---
layout: post
title:  "1079. Letter Tile Possibilities"
date: 2019-07-18 21:01:00 -0400
categories: articles
---

You have a set of tiles, where each tile has one letter tiles[i] printed on it.  Return the number of possible non-empty sequences of letters you can make.

Example 1:
```
Input: "AAB"
Output: 8
Explanation: The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".
```
Example 2:
```
Input: "AAABBC"
Output: 188
```

Note:
```
1 <= tiles.length <= 7
tiles consists of uppercase English letters.
```
```c++
// Accepted! 2019-07-20
class Solution {
public:
    int res = 0;
    int numTilePossibilities(string tiles) {
        int max_size = tiles.size();
        unordered_map<char, int> record;
        for ( auto i : tiles) record[i]++;
        helper(0, max_size, record);
        return res;
    }
    
    void helper(int cur, int max_size, unordered_map<char, int>& record) {
            if ( cur == max_size ) return;
            for ( int i = 0; i < 26; i++ ) {
                if ( record['A'+i] > 0 ) {
                    res++;
                    record['A'+i]--;
                    helper(cur+1, max_size, record);
                    record['A'+i]++;
                }
            }
        }
};
```
```c++
class Solution {
public:
    int ans = 0;
    int cnt[27];
    inline int numTilePossibilities(string& tiles) {
        for(int i = 0; i < tiles.size(); ++i){
            cnt[tiles[i] - 'A']++;
        }
        calc(0, tiles.size());
        return ans;
    }
    inline void calc(int sz, int max_sz){
        if(sz == max_sz) return;
        for(int i = 0; i < 26; ++i){
            if(cnt[i] > 0){
                cnt[i]--;
                ans++;
                calc(sz + 1, max_sz);
                cnt[i]++;
            }
        }
    }
};
```
```c++
class Solution {
public:
    int ans = 0;
    int cnt[27];
    int numTilePossibilities(string& tiles) {
        for(int i = 0; i < tiles.size(); ++i){
            cnt[tiles[i] - 'A']++;
        }
        calc(0, tiles.size());
        return ans;
    }
    void calc(int sz, int max_sz){
        if(sz == max_sz) return;
        for(int i = 0; i < 26; ++i){
            if(cnt[i] > 0){
                cnt[i]--;
                ans++;
                calc(sz + 1, max_sz);
                cnt[i]++;
            }
        }
    }
};
```