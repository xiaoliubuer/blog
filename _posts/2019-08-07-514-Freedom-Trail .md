---
layout: post
title:  "514. Freedom Trail"
date: 2019-08-07 20:10:00 -0400
categories: articles
---
In the video game Fallout 4, the quest "Road to Freedom" requires players to reach a metal dial called the "Freedom Trail Ring", and use the dial to spell a specific keyword in order to open the door.

Given a string ring, which represents the code engraved on the outer ring and another string key, which represents the keyword needs to be spelled. You need to find the minimum number of steps in order to spell all the characters in the keyword.

Initially, the first character of the ring is aligned at 12:00 direction. You need to spell all the characters in the string key one by one by rotating the ring clockwise or anticlockwise to make each character of the string key aligned at 12:00 direction and then by pressing the center button.

At the stage of rotating the ring to spell the key character key[i]:

You can rotate the ring clockwise or anticlockwise one place, which counts as 1 step. The final purpose of the rotation is to align one of the string ring's characters at the 12:00 direction, where this character must equal to the character key[i].
If the character key[i] has been aligned at the 12:00 direction, you need to press the center button to spell, which also counts as 1 step. After the pressing, you could begin to spell the next character in the key (next stage), otherwise, you've finished all the spelling.
Example:

```
Input: ring = "godding", key = "gd"
Output: 4
```
Explanation:
```
For the first key character 'g', since it is already in place, we just need 1 step to spell this character. 
For the second key character 'd', we need to rotate the ring "godding" anticlockwise by two steps to make it become "ddinggo".
Also, we need 1 more step for spelling.
So the final output is 4.
```
Note:
```
Length of both ring and key will be in range 1 to 100.
There are only lowercase letters in both strings and might be some duplcate characters in both strings.
It's guaranteed that string key could always be spelled by rotating the string ring.
```
```c++
long long dp[101][101];
class Solution {
public:
    long long find(int posRing,int posKey,string &ring, string &key){
        int sz = ring.size();
        if(posKey>= key.size()) return 0;
        if(dp[posRing][posKey] != -1) return dp[posRing][posKey];
        dp[posRing][posKey] = INT_MAX;
        for(int i=0;i<sz;i++){
            int pos = (posRing+i)%sz;
            if(ring[pos] == key[posKey]){
                int steps = min(i,sz-i)+1;
                dp[posRing][posKey] = 
                    min(dp[posRing][posKey], find(pos,posKey+1,ring,key)+steps);
            }
        }
        return dp[posRing][posKey];
    }
    
    int findRotateSteps(string ring, string key) {
        memset(dp,-1,sizeof(dp));
        return find(0,0,ring,key);
    }
};
```

```c++
class Solution {
    
    // find the minimun cost to stop at ring[j]
    int getMinDistance(vector<pair<int, int>>& cur, int j, int len) {
        int le, ri;
        if (j < cur[0].first || j >= cur.back().first) {
            
            // j is out of bound of cur, so the leftmost and rightmost locations are nearest
            ri = 0;
            le = cur.size()-1;
        }
        else {
            
            // binary search to find location with smallest distance
            le = 0, ri = cur.size()-1;
            while (le < ri) {
                int mi = le + (ri - le) / 2;
                if (j <= cur[mi].first)
                    ri = mi;
                else if (j >= cur[mi+1].first)
                    le = mi+1;
                else {
                    le = mi;
                    break;
                }
            }
            ri = le + 1;
        }
        
        // find the minimun cost
        int lo = cur[le].first, ro = cur[ri].first;
        int lecost = min(abs(j-lo), len - abs(j-lo)) + cur[le].second;
        int ricost = min(abs(j-ro), len - abs(j-ro)) + cur[ri].second;
        return min(lecost, ricost);
    }
    
public:
    int findRotateSteps(string ring, string key) {
        int N = key.size(), M = ring.size();
        
        // use 1D array to apply DP
        //   cur, _cur are always in ascending order, which support binary search
        vector<pair<int, int>> cur, _cur;
        for (int j=0; j<M; j++) 
            if (key[0] == ring[j])
                cur.push_back( {j, min(j, M-j)} );
        
        for (int i=1; i<N; i++) {
            for (int j=0; j<M; j++) {
                if (ring[j] == key[i]) 
                    _cur.push_back( {j, getMinDistance(cur, j, M)} );
            }
            cur = _cur;
            _cur.clear();
        }
        
        int cost = INT_MAX;
        for (auto p : cur) 
            cost = min(cost, p.second);
        return cost + N;
    }
};
```