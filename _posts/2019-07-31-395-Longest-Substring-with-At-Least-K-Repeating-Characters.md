---
layout: post
title:  "395. Longest Substring with At Least K Repeating Characters"
date: 2019-07-31 22:02:00 -0400
categories: articles
---
Find the length of the longest substring T of a given string (consists of lowercase letters only) such that every character in T appears no less than k times.

Example 1:
```
Input:
s = "aaabb", k = 3

Output:
3
```
The longest substring is "aaa", as 'a' is repeated 3 times.
```
Example 2:
```
Input:
s = "ababbc", k = 2

Output:
5
```
The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```c++
class Solution {
public:
int helper(const string &s, int k, int ll, int rr)
{
	unordered_map<char, int> mp;
	for (int i = ll;i <= rr;i++) {
		mp[s[i]]++;
	}
	int ans = 0;
	int i, j;
	for (i = ll, j = ll;i <= rr;i++) {
		if (mp[s[i]] < k) {
			ans = max(ans, helper(s, k, j, i - 1));
			j = i + 1;
		}
	}
	if(j==ll)
		return rr - ll + 1;
	return max(ans, helper(s, k, j, rr));
}

int longestSubstring(string s, int k) {
	return helper(s, k, 0, s.size() - 1);
}
};
```
```c++
class Solution {
public:
    int longestSubstring(string s, int k) {
        //cout << s[0] << endl;
        if(s.length()==0 || k > s.length())
            return 0;
        
        int pos[26] = { 0 };
        unordered_map<char, int> map;
        for(int i = 0; i < s.length(); i++){
            map[s[i]]++;
            pos[s[i]-'a'] = i;
            // cout << s[i] << " " << map[s[i]] << endl;
        }
        
        int idx = 0;
        cout << idx << " " << map[s[idx]] << " " << k <<  endl;
        while(map[s[idx]] >= k && idx < s.length()){
            cout << idx << " " << map[s[idx]] << endl;
            idx++;
        }
        if(idx == 0)
            idx = pos[s[0]-'a'];
        
        if(idx == s.length())
            return s.length();
        
        cout << s[0] << " " << idx << endl;
        
        int left = longestSubstring(s.substr(0,idx), k);
        
        int right = longestSubstring(s.substr(idx+1), k);
        
        return max(left, right);
    }
};
```