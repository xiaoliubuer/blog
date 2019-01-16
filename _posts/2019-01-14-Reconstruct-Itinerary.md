---
layout: post
title:  "** 332. Reconstruct Itinerary"
date: 2019-01-14 20:27:23 -0400
categories: articles
---
Given a list of airline tickets represented by pairs of departure and arrival airports [from, to], reconstruct the itinerary in order. All of the tickets belong to a man who departs from JFK. Thus, the itinerary must begin with JFK.

Note:

If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
All airports are represented by three capital letters (IATA code).
You may assume all tickets form at least one valid itinerary.
Example 1:
```
Input: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
Output: ["JFK", "MUC", "LHR", "SFO", "SJC"]
```
Example 2:
```
Input: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"].
             But it is larger in lexical order.
```
# Funciton signature
```c++
class Solution {
public:
    vector<string> findItinerary(vector<pair<string, string>> tickets) {
        
    }
};
```
# 题意
给一个list的pair表示出发和到达。
# 想法
因为要返回这个顺序，所以要先从JFK找起。找到了之后再找它连着的。那么就是一个DFS的过程。
那么如何DFS呢？？简单想就是把它们转化为矩阵。在矩阵里进行DFS。
所以想先用set，找到有多少个地方。
然后创建n * n的矩阵，然后还要给所有的string排序，然后依次遍历输入的tickets，按照他们的出发和到达
的地点对应的放入矩阵。或者其他数据结构。因为是有方向的。所以用图？其实图的表现形式就是这两种方法，pair
或者是matrix。
形成了matrix之后，就行JFK，最好把它放在0位置，这样一次遍历，DFS，把所有的点都找到。最后输出那个数组。

# Shame answer
```c++
class Solution {
public:
    vector<string> findItinerary(vector<pair<string, string>> tickets) {
        unordered_map<string, multiset<string> > G; // 这里太重要啦！！！ multiset，c++自带的可以排序的库 太有用了！！
        for (auto t : tickets) {
            G[t.first].insert(t.second);
        }
        vector<string> res;
        vector<string> s;
        s.push_back("JFK");
        while (!s.empty()) {
            string port = s.back();
            if (G[port].empty()) {
                res.push_back(port);
                s.pop_back();
            } else {
                s.push_back(*G[port].begin());
                G[port].erase(G[port].begin());
            }
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
# Shame answer 2
```c++
class Solution {
public:
    
    void GetPath(
        string src,
        map<string, priority_queue<string, vector<string>, std::greater<string>>>& tix,
        vector<string>& result) {
        priority_queue<string, vector<string>, std::greater<string>>& dests = tix[src];
        while (dests.size()){
            string dest = dests.top();
            dests.pop();
            GetPath(dest, tix, result);    
        }
        result.push_back(src);
    }
    
    vector<string> findItinerary(vector<pair<string, string>> tickets) {
        vector<string> result;
        if (tickets.empty()){
            return result;
        }
        
        map<string, priority_queue<string, vector<string>, std::greater<string>>> hops;
        for (const pair<string,string>& ticket : tickets){
            hops[ticket.first].push(ticket.second);
        }
        
        GetPath("JFK", hops, result); 
        
        return vector<string>(result.rbegin(), result.rend());
    }
};
```