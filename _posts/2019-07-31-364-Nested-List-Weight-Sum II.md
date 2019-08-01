---
layout: post
title:  "364. Nested List Weight Sum II"
date: 2019-07-31 20:11:00 -0400
categories: articles
---

Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Different from the previous question where weight is increasing from root to leaf, now the weight is defined from bottom up. i.e., the leaf level integers have weight 1, and the root level integers have the largest weight.

Example 1:
```
Input: [[1,1],2,[1,1]]
Output: 8 
```
Explanation: Four 1's at depth 1, one 2 at depth 2.
Example 2:
```
Input: [1,[4,[6]]]
Output: 17 
```
Explanation: One 1 at depth 3, one 4 at depth 2, and one 6 at depth 1; 1 * 3 + 4 * 2 + 6 * 1 = 17.

```c++
class Solution {
public:
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        vector<int> deepSum(1, 0);
        depthSumInverseHelper(nestedList,deepSum, 1);
        std::reverse(deepSum.begin() + 1, deepSum.end());
        int sum = 0;
        for (int i = 0; i < deepSum.size(); i++) {
            sum += i * (deepSum[i]);
        }
        return sum;
    }
    
    void depthSumInverseHelper(vector<NestedInteger>& nestedList,vector<int> & deepSum, int deepth) {
        if (deepth >= deepSum.size()) {
            deepSum.push_back(0);
        }
        
        for(int i = 0; i < nestedList.size(); i++) {
            if (nestedList[i].isInteger()) {
                deepSum[deepth] += nestedList[i].getInteger();
            } 
            else 
            {
                depthSumInverseHelper(nestedList[i].getList(), deepSum, deepth + 1);
            }
        }
    }
};
```
```c++
class Solution {
public:
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        
        vector<pair<int, int>> ans;
        int max_depth = 1;
        int result = 0;
        helper(nestedList, 1, max_depth, ans);
        for (const auto &p : ans) {
            result += (p.first * (max_depth - p.second + 1));
        }
        return result;
    }
    
    void helper(vector<NestedInteger>& nl, int depth, int &max_depth, vector<pair<int, int>> &ans) {
        if (nl.size() == 0) return;
        max_depth = max(depth, max_depth);
        for (auto &n : nl) {
            if (n.isInteger()) {
                ans.emplace_back(n.getInteger(), depth);
                continue;
            }
            else helper(n.getList(), depth+1, max_depth, ans);
        }
    }
};
```
```c++
class Solution {
public:
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        vector<int> result;
        for(auto ni : nestedList) {
            dfs(ni, 0, result);
        }
        //post processing 
        int sum = 0;
        for(int i = result.size()-1,level = 1; i >=0; i--, level++) {
            sum += result[i]*level;
        }
        
        return sum;
    }
    
private:
    void dfs(NestedInteger &ni, int depth, vector<int> & result) {
        if(result.size() < depth+1) result.resize(depth+1);
        if(ni.isInteger()) {
            result[depth] += ni.getInteger();
        } else {
            for(auto n_ni : ni.getList()) {
                dfs(n_ni, depth+1, result);
            }
        }
        
    }
    
    
};
```