---
layout: post
title:  "282. Expression Add Operators"
date: 2019-07-28 19:50:00 -0400
categories: articles
---
Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

Example 1:
```
Input: num = "123", target = 6
Output: ["1+2+3", "1*2*3"] 
```
Example 2:
```
Input: num = "232", target = 8
Output: ["2*3+2", "2+3*2"]
```
Example 3:
```
Input: num = "105", target = 5
Output: ["1*0+5","10-5"]
```
Example 4:
```
Input: num = "00", target = 0
Output: ["0+0", "0-0", "0*0"]
```
Example 5:
```
Input: num = "3456237490", target = 9191
Output: []
```
```c++
class Solution {
public:
    //solution 2, read solution 1 first
    vector<string> addOperators(string num, int target) {
        vector<string> result;
        string path(num.size() * 2, '0');
        if(!num.empty())
            helper(num, 0, target, path, 0, result, 0, 0);
        return result;
    }
    
    void helper(string &num, int idx, int target, string &path, int len, vector<string> &result, long cur, long pre) {
        if(idx == num.size() && cur == target)
            result.push_back(path.substr(0, len));
        
        long val = 0;
        int pos = idx == 0 ? len : len++;
        for(int i = idx; i < num.size(); i++) {
            if(i != idx && num[idx] == '0')
                break;
            path[len++] = num[i];
            val = val * 10 + num[i] - '0';
            if(idx == 0)
                helper(num, i+1, target, path, len, result, val, val);
            else {
                path[pos] = '+';
                helper(num, i+1, target, path, len, result, cur + val, val);
                path[pos] = '-';
                helper(num, i+1, target, path, len, result, cur - val, -val);
                path[pos] = '*';
                helper(num, i+1, target, path, len, result, cur - pre + pre * val, pre * val);
            }
        }
    }
};
```
```c++
class Solution {
private:
    // cur: { string } expression generated so far.
    // pos: { int }    current visiting position of num.
    // cv:  { long }   cumulative value so far.
    // pv:  { long }   previous operand value.
    // op:  { char }   previous operator used.
    void dfs(std::vector<string>& res, const string& num, const int target, string cur, int pos, const long cv, const long pv, const char op) {
        if (pos == num.size() && cv == target) {
            res.push_back(cur);
        } else {
            for (int i=pos+1; i<=num.size(); i++) {
                string t = num.substr(pos, i-pos);
                long now = stol(t);
                if (to_string(now).size() != t.size()) continue;
                dfs(res, num, target, cur+'+'+t, i, cv+now, now, '+');
                dfs(res, num, target, cur+'-'+t, i, cv-now, now, '-');
                dfs(res, num, target, cur+'*'+t, i, (op == '-') ? cv+pv - pv*now : ((op == '+') ? cv-pv + pv*now : pv*now), pv*now, op);
            }
        }
    }

public:
    vector<string> addOperators(string num, int target) {
        vector<string> res;
        if (num.empty()) return res;
        for (int i=1; i<=num.size(); i++) {
            string s = num.substr(0, i);
            long cur = stol(s);
            if (to_string(cur).size() != s.size()) continue;
            dfs(res, num, target, s, i, cur, cur, '#');         // no operator defined.
        }

        return res;
    }
};
```