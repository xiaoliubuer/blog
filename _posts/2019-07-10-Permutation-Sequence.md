---
layout: post
title:  "60. Permutation Sequence"
date: 2019-07-10 20:25:00 -0400
categories: articles
---	

The set [1,2,3,...,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for n = 3:

"123"
"132"
"213"
"231"
"312"
"321"
Given n and k, return the kth permutation sequence.

Note:

Given n will be between 1 and 9 inclusive.
Given k will be between 1 and n! inclusive.
Example 1:

Input: n = 3, k = 3
Output: "213"
Example 2:

Input: n = 4, k = 9
Output: "2314"

```c++
class Solution {
public:
    string getPermutation(int n, int k) {
        vector<int> visited(10, 0);
        return genPermute(n, k, visited);
    }
    
    string genPermute(int len, int k, vector<int>& visited) {
        string result;
        if (len == 1) {
            for (int i = 1; i <= 9; i++) {
                if (!visited[i])  {
                    visited[i] = 1;
                    result += '0' + i;
                    return result;
                }
            }
        } else {
            int nextLenFact = 1;
            for (int i = 1; i < len; i++) {
                nextLenFact *= i;
            }
            // decrease k until k <= nextLenFact
            for (int i = 1; i <= 9; i++) {
                if (!visited[i])  {
                    if (k <= nextLenFact) {
                        visited[i] = 1;
                        result += '0' + i;
                        return result + genPermute(len - 1, k, visited);
                    } else {
                        k -= nextLenFact;
                    }
                }
            }
        }
        return result;
    }
};
```
```c++
class Solution {
public:
    string getPermutation(int n, int k) {
        int pTable[10] = {1};        //factorial pre-calculated
        for(int i = 1; i < 10; i++)  pTable[i] = i * pTable[i-1];
        
        vector<char> numSet;         //in-order num set to fetch numbers
        for(int i = 0; i < n; i++) numSet.push_back('0'+i+1);
        
        string ans = "";
        int k1 = ( k - 1 ) % pTable[n];  //if k > n!. (k-1) for zero-based index
        int n1 = n;
        
        while(n1 > 0) {
            int numPermutation = k1 / pTable[n1-1];
            ans += numSet[numPermutation];
            numSet.erase(numSet.begin() + numPermutation);
            k1 %= pTable[n1-1];
            n1--;
        }
        return ans;
    }
};
```