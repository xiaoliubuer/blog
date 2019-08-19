---
layout: post
title:  "664. Strange Printer"
date: 2019-08-18 16:26:00 -0400
categories: articles
---
There is a strange printer with the following two special requirements:

The printer can only print a sequence of the same character each time.
At each turn, the printer can print new characters starting from and ending at any places, and will cover the original existing characters.
Given a string consists of lower English letters only, your job is to count the minimum number of turns the printer needed in order to print it.

Example 1:
```
Input: "aaabbb"
Output: 2
```
Explanation: Print "aaa" first and then print "bbb".
Example 2:
```
Input: "aba"
Output: 2
```
Explanation: Print "aaa" first and then print "b" from the second place of the string, which will cover the existing character 'a'.
Hint: Length of the given string will not exceed 100.

```c++
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                dp[i][j] = (i == j) ? 1 : (1 + dp[i + 1][j]);
                for (int k = i + 1; k <= j; ++k) {
                    if (s[k] == s[i]) dp[i][j] = min(dp[i][j], dp[i + 1][k - 1] + dp[k][j]);
                }
            }
        }
        return (n == 0) ? 0 : dp[0][n - 1];
    }
};
```
```c++
class Solution {
public:
    int strangePrinter(string s) {
        if (s.empty()) {
            return 0;
        }

        int n = 1;
        for (const char c : s) {
            if (s[n - 1] != c) {
                s[n] = c;
                n++;
            }
        }
        s.resize(n);

        if (n <= 2) {
            return n;
        }

        vector<vector<int>> r(n + 1);
        for (int count = 0; count <= n; count++) {
            r[count] = vector<int>(n + 1 - count);
        }

        fill(r[0].begin(), r[0].end(), 0);
        fill(r[1].begin(), r[1].end(), 1);

        for (int count = 2; count <= n; count++) {
            for (int start = 0; start < n + 1 - count; start++) {
                int steps = r[count - 1][start + 1] + 1;
                for (int i = 2; i < count; i++) {
                    if (s[start + i] == s[start]) {
                        steps = min(steps, r[i][start] + r[count - i - 1][start + i + 1]);
                    }
                }
                r[count][start] = steps;
            }
        }

        return r[n][0];
    }
};
```