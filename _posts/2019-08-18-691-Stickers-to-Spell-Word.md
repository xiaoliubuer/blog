---
layout: post
title:  "691. Stickers to Spell Word"
date: 2019-08-18 18:05:00 -0400
categories: articles
---
We are given N different types of stickers. Each sticker has a lowercase English word on it.

You would like to spell out the given target string by cutting individual letters from your collection of stickers and rearranging them.

You can use each sticker more than once if you want, and you have infinite quantities of each sticker.

What is the minimum number of stickers that you need to spell out the target? If the task is impossible, return -1.

Example 1:
```
Input:

["with", "example", "science"], "thehat"
Output:

3
```
Explanation:
```
We can use 2 "with" stickers, and 1 "example" sticker.
After cutting and rearrange the letters of those stickers, we can form the target "thehat".
Also, this is the minimum number of stickers necessary to form the target string.
```
Example 2:
```
Input:

["notice", "possible"], "basicbasic"
Output:

-1
```
Explanation:
```
We can't form the target "basicbasic" from cutting letters from the given stickers.
```
Note:
```
stickers has length in the range [1, 50].
stickers consists of lowercase English words (without apostrophes).
target has length in the range [1, 15], and consists of lowercase English letters.
In all test cases, all words were chosen randomly from the 1000 most common US English words, and the target was chosen as a concatenation of two random words.
The time limit may be more challenging than usual. It is expected that a 50 sticker test case can be solved within 35ms on average.
```

```c++
class Solution {
public:
    int minStickers(vector<string>& stickers, string target) {
        int n = stickers.size(), sz = target.size();
        vector<vector<int>> v(n, vector<int>(26, 0));
        for(int i = 0; i < n; ++i) {
            for(char c : stickers[i]) {
                ++v[i][c - 'a'];
            }
        }
        unordered_map<string, int> m{ {"", 0}};
        function<int(const string&)> dfs = [&](const string& target) {
            if(m.count(target)) return m[target];
            vector<int> cnt(26, 0);
            for(char c : target) ++cnt[c - 'a'];
            int res = sz + 1;
            for(int i = 0; i < n; ++i) {
                if(v[i][target[0] - 'a']) {
                    string str;
                    for(int j = 0; j < 26; ++j) {
                        if(cnt[j] - v[i][j] > 0) str += string(cnt[j] - v[i][j], 'a' + j);
                    }
                    res = min(res, 1 + dfs(str));
                }
            }
            m[target] = res;
            return res;
        };
        
        int res = dfs(target);
        return res == sz + 1 ? -1 : res;
    }
};
```
```c++
class Solution {
public:
    int minStickers(vector<string>& stickers, string target) {
        int len = target.length();
        vector<int> dp(1<<len, -1);
        dp[0] = 0;
        
        for(int i = 0; i < (1<< len); i ++) {
            if(dp[i] == -1) continue;
            
            for(auto& str: stickers) {
                int nowState = i;
                for(auto& ch: str) {
                    for(int j = 0; j < len; j ++) {
                        if( ((nowState>>j) & 1) == 1) continue;
                        if(ch == target[j]) {
                            nowState |= (1 << j);
                            break;
                        }
                    }
                }
                if(dp[nowState] == -1 || dp[nowState] > dp[i] + 1) {
                    dp[nowState] = dp[i]+ 1;
                }
            }
        }
        return dp[(1 << len) - 1];
    }
};
```
```c++
class Solution {
public:
    
    unordered_map<string,unordered_map<int,int>>mp = {};
    unordered_map<int,unordered_map<char,int>>mpp;
    int func(vector<string>&stickers, string target,int index,int n){
        if(target.empty()){
            return 0;
        }
        if(index == n){
            return -1;
        }
        if(mp.count(target) && mp[target].count(index)){
            return mp[target][index];
        }
        int res = INT_MAX;
        //cout << target << endl;
        for(int i = index; i<n; i++){
            string temp;
            //cout << i << endl;
            if(!mpp[i].count(target[0]) || mpp[i][target[0]] == 0){
                continue;
            }
            //cout << i << endl;
            unordered_map<char,int>m;
            for(int j = 0;j<target.size();j++){
                if(m[target[j]] < mpp[i][target[j]]){
                    m[target[j]]++;
                }else{
                    temp.push_back(target[j]);
                }
            }
            int r = func(stickers,temp,index,n);
            if(r != -1){
                res = min(res,r+1);
            }
        }
        mp[target][index] = res ==INT_MAX ? -1 : res;
        return res == INT_MAX ? -1 : res;
    }
    
    int minStickers(vector<string>& stickers, string target) {
        int n = stickers.size();
        for(int i = 0;i<stickers.size();i++){
            for(auto j : stickers[i]){
                mpp[i][j]++;
            }
        }
        int res = func(stickers,target,0,n);
        return res == INT_MAX ? -1 : res;
    }
};
```
```c++
class Solution {
public:
    int helper(vector<string>& stickers, string& target, unordered_map<int, int>& cache, int mask) {
        if (!mask) { return 0; } // If mask == 0, return 0;
        
        auto it = cache.find(mask);
        if (it != cache.end()) {
            return (*it).second; 
        }
        
        int N = target.size();
        int result = INT_MAX;
        for (string s : stickers) {
            int new_mask = mask;
            for (char c : s) {
                for (int i=0; i < N; ++i) {
                    // Check if a character in sticker matches a not-yet matched char in target.
                    if (target[i] == c && ((new_mask >> (N - i - 1)) & 1)) {
                        new_mask &= ~(1 << (N-i-1));
                        break; // This is important, otherwise one character can be matched to many in target.
                    }
                }
            }
            if (new_mask != mask) {
                int sub_problem_result = helper(stickers, target, cache, new_mask);
                if (sub_problem_result == -1) { cache[mask] = -1; return -1; }
                
                result = min(result, 1 + sub_problem_result);
            }
        }
        cache[mask] = result == INT_MAX ? -1 : result;
        return cache[mask];
    }
    
    int minStickers(vector<string>& stickers, string target) {
        int mask = (1 << target.size()) - 1;
        unordered_map<int, int> result;
        return helper(stickers, target, result, mask);
    }
};
```