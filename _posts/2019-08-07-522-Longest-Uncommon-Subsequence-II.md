---
layout: post
title:  "522. Longest Uncommon Subsequence II"
date: 2019-08-07 20:27:00 -0400
categories: articles
---
Given a list of strings, you need to find the longest uncommon subsequence among them. The longest uncommon subsequence is defined as the longest subsequence of one of these strings and this subsequence should not be any subsequence of the other strings.

A subsequence is a sequence that can be derived from one sequence by deleting some characters without changing the order of the remaining elements. Trivially, any string is a subsequence of itself and an empty string is a subsequence of any string.

The input will be a list of strings, and the output needs to be the length of the longest uncommon subsequence. If the longest uncommon subsequence doesn't exist, return -1.

Example 1:
```
Input: "aba", "cdc", "eae"
Output: 3
```
Note:
```
All the given strings' lengths will not exceed 10.
The length of the given list will be in the range of [2, 50].
```
```c++
class Solution {
public:
    int findLUSlength(vector<string>& strs) {
        if (strs.empty()) return -1;
        int rst = -1;
        for(auto i = 0; i < strs.size(); i++) {
            int j = 0;
            for(j=0; j < strs.size(); j++) {
                if(i==j) continue;
                if(LCS(strs[i], strs[j])) break;  // strs[j] is a subsequence of strs[i]
            }
            // strs[i] is not any subsequence of the other strings.
            if(j==strs.size()) rst = max(rst, static_cast<int>(strs[i].size()));
        }
        return rst;
    }
    // iff a is a subsequence of b
    bool LCS(const string& a, const string& b) {
        if (b.size() < a.size()) return false;
        int i = 0;
        for(auto ch: b) {
            if(i < a.size() && a[i] == ch) i++;
        }
        return i == a.size();
    }
};
```
```c++
class Solution {
public:
bool isSubSeq(string &l, string &r) {
	int i = 0, j = 0;
	while (l[i] && r[j]) {
		if (l[i] == r[j]) i++;
		j++;
	}
	return l[i] == 0;
}

int findLUSlength(vector<string>& strs) {
	sort(strs.begin(), strs.end(), [](const string &l, const string &r) {
		if (l.size() == r.size()) return l < r;
		return l.size() < r.size();
	});
	int n = strs.size() - 1, i;
	while (n-- > 0) {
		if (strs[n] != strs[n + 1]) break;
		else {
			i = n--;
            while (n >= 0 && isSubSeq(strs[n], strs[i])) n--;
		}
	}
	return (n >= -1) ? strs[n + 1].size() : -1;
}
};
```
```c++
class Solution {
public://b is a's sub
    bool is_subsequence(const string& t, const string& s) {
        if(s.size() < t.size()) return false;
        int i = 0;
        for(char c: s) {
            if(i < t.size() && t[i] == c) ++i;
        };
        return i == t.size();
    }
    int findLUSlength(vector<string>& strs) {
        if(strs.empty()) return -1;
        int ans = -1;
        for(int i = 0; i < strs.size(); ++i) {
            int j = 0;
            for(j = 0; j < strs.size(); ++j) {
                if(i == j) continue;
                if(is_subsequence(strs[i], strs[j])) break;
            };
            if(j == strs.size()) ans = std::max(ans, (int)(strs[i].size()));
        };
        return ans;
    }
};
```