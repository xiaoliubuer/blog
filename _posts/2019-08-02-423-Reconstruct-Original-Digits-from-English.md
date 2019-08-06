---
layout: post
title:  "423. Reconstruct Original Digits from English"
date: 2019-08-02 19:24:00 -0400
categories: articles
---
Given a non-empty string containing an out-of-order English representation of digits 0-9, output the digits in ascending order.

Note:
Input contains only lowercase English letters.
Input is guaranteed to be valid and can be transformed to its original digits. That means invalid inputs such as "abc" or "zerone" are not permitted.
Input length is less than 50,000.
Example 1:
```
Input: "owoztneoer"

Output: "012"
```
Example 2:
```
Input: "fviefuro"

Output: "45"
```

```c++
class Solution {
public:
    string originalDigits(string s) {
    vector<int> count(26, 0); 
    for (auto c: s)
        count[c - 'a'] ++;	
    vector<int> num(10, 0);
  
    num[0] = count['z' - 'a'];
    num[2] = count['w' - 'a'];
    num[4] = count['u' - 'a'];
    num[8] = count['g' - 'a'];
    num[5] = count['f' - 'a'] - num[4];
    num[7] = count['v' - 'a'] - num[5];
    num[3] = count['t' - 'a'] - num[2] - num[8];
    
    num[6] = count['s' - 'a'] - num[7];
    num[1] = count['o' - 'a'] - num[0] - num[2] - num[4];
    num[9] = count['i' - 'a'] - num[5] - num[6] - num[8];
    string ans = "";
    for (int i = 0; i < 10; i ++) {
        ans.append(num[i], '0' + i);
    }
    return ans;
}
};
```
```c++
class Solution {
public:
    string originalDigits(string s) {
        string res;
        char dict[10][6] = {"zero", "wto", "ufor", "xis", "geiht", "one", "three", "five", "seven", "inne"};
        char num[10] = {'0', '2', '4', '6', '8', '1', '3', '5', '7', '9'};
        uint hash[26] = {0};
        for (auto c : s) hash[c - 'a']++;
        for (uint i = 0; i < 10; i++) {
            uint cnt = hash[dict[i][0] - 'a'];
            for (uint j = 0, c; c = dict[i][j]; j++) hash[c - 'a'] -= cnt;
            res.insert(res.end(), cnt, num[i]);
        }
        sort(res.begin(), res.end());
        return res;
    }
};
```