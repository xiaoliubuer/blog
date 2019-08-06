---
layout: post
title:  "465. Optimal Account Balancing"
date: 2019-08-05 19:46:00 -0400
categories: articles
---
A group of friends went on holiday and sometimes lent each other money. For example, Alice paid for Bill's lunch for $10. Then later Chris gave Alice $5 for a taxi ride. We can model each transaction as a tuple (x, y, z) which means person x gave person y $z. Assuming Alice, Bill, and Chris are person 0, 1, and 2 respectively (0, 1, 2 are the person's ID), the transactions can be represented as [[0, 1, 10], [2, 0, 5]].

Given a list of transactions between a group of people, return the minimum number of transactions required to settle the debt.

Note:

A transaction will be given as a tuple (x, y, z). Note that x ≠ y and z > 0.
Person's IDs may not be linear, e.g. we could have the persons 0, 1, 2 or we could also have the persons 0, 2, 6.
Example 1:
```
Input:
[[0,1,10], [2,0,5]]

Output:
2
```
Explanation:
```
Person #0 gave person #1 $10.
Person #2 gave person #0 $5.
```
Two transactions are needed. One way to settle the debt is person #1 pays person #0 and #2 $5 each.
Example 2:
```
Input:
[[0,1,10], [1,0,1], [1,2,5], [2,0,5]]

Output:
1
```
Explanation:
```
Person #0 gave person #1 $10.
Person #1 gave person #0 $1.
Person #1 gave person #2 $5.
Person #2 gave person #0 $5.
```
Therefore, person #1 only need to give person #0 $4, and all debt is settled.

```c++
class Solution {
private:
    int dfs(vector<int>& debts, int person) {
        while (person < debts.size() && debts[person] == 0)
            person++;
        if (person == debts.size())
            return 0;
        
        int minTxns = INT_MAX;
        bool positive = debts[person] > 0;
        int prevDebt = 0;
        for (int nextPerson = person + 1; nextPerson < debts.size(); nextPerson++) {
            if (debts[nextPerson] != prevDebt && (debts[nextPerson] > 0) != positive) {
                debts[nextPerson] += debts[person];
                minTxns = min(minTxns, 1 + dfs(debts, person + 1));
                prevDebt = debts[nextPerson] -= debts[person];
            }
        }
        
        return (minTxns == INT_MAX) ? 0 : minTxns;
    }
    
public:
    int minTransfers(vector<vector<int>>& transactions) {
        unordered_map<int, int> balances;
        for (auto &transaction: transactions) {
            int giver = transaction[0];
            int taker = transaction[1];
            int amount = transaction[2];
            balances[giver] -= amount;
            balances[taker] += amount;
        }
        
        vector<int> debts;
        for (auto it = balances.begin(); it != balances.end(); it++) {
            int amount = it->second;
            if (amount != 0)
                debts.push_back(amount);
        }
        sort(debts.begin(), debts.end());
        return dfs(debts, 0);
    }
};
```


```c++
class Solution {
public:
    int minTransfers(vector<vector<int>>& transactions) {
        unordered_map<int, int> m;
        for (auto t : transactions) {
            m[t[0]] -= t[2];
            m[t[1]] += t[2];
        }
        vector<int> accnt(m.size());
        int cnt = 0;
        for (auto a : m) {
            if (a.second != 0) accnt[cnt++] = a.second;
        }
        return helper(accnt, 0, 0);
    }
    int helper(vector<int>& accnt, int start, int num) {
        while (start < accnt.size() - 1 && accnt[start] == 0) ++start;
        if (start == accnt.size() - 1) {
            return 0;
        }
        int res = INT_MAX;
        for (int i = start + 1; i < accnt.size(); ++i) {
            if (accnt[i] * accnt[start] < 0) {
                accnt[i] += accnt[start];
                res = min(res, helper(accnt, start + 1, num) + 1);
                accnt[i] -= accnt[start];
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int minTransfers(vector<vector<int>>& transactions) {
        unordered_map<int, int> mp;
        for (auto x : transactions) {
            mp[x[0]] -= x[2];
            mp[x[1]] += x[2];
        }
        vector<int> in;
        vector<int> out;
        for (auto x : mp) {
            if (x.second < 0) out.push_back(-x.second);
            else if (x.second > 0) in.push_back(x.second);
        }
        int amount = 0;
        for (auto x : in) amount += x;
        if (amount == 0) return 0;
        int res = (int)in.size() + (int)out.size() - 1;
        dfs(in, out, 0, 0, amount, 0, res);
        return res;
    }
    
    void dfs(vector<int> &in, vector<int> &out, int i, int k, 
             int amount, int step, int &res) {
        if (step >= res) return;
        if (amount == 0) {
            res = step;
            return;
        }
        if (in[i] == 0) {
            ++i;
            k = 0;
        }
        for (int j = k; j < out.size(); j++) {
            int dec = min(in[i], out[j]);
            if (dec == 0) continue;
            in[i] -= dec;
            out[j] -= dec;
            dfs(in, out, i, j + 1, amount - dec, step + 1, res);
            in[i] += dec;
            out[j] += dec;
        }
    }
};
```