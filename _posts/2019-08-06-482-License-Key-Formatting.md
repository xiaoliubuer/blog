---
layout: post
title:  "482. License Key Formatting"
date: 2019-08-06 20:01:00 -0400
categories: articles
---
You are given a license key represented as a string S which consists only alphanumeric character and dashes. The string is separated into N+1 groups by N dashes.

Given a number K, we would want to reformat the strings such that each group contains exactly K characters, except for the first group which could be shorter than K, but still must contain at least one character. Furthermore, there must be a dash inserted between two groups and all lowercase letters should be converted to uppercase.

Given a non-empty string S and a number K, format the string according to the rules described above.

Example 1:
```
Input: S = "5F3Z-2e-9-w", K = 4

Output: "5F3Z-2E9W"

Explanation: The string S has been split into two parts, each part has 4 characters.
Note that the two extra dashes are not needed and can be removed.
```
Example 2:
```
Input: S = "2-5g-3-J", K = 2

Output: "2-5G-3J"

Explanation: The string S has been split into three parts, each part has 2 characters except the first part as it could be shorter as mentioned above.
```
Note:
```
The length of string S will not exceed 12,000, and K is a positive integer.
String S consists only of alphanumerical characters (a-z and/or A-Z and/or 0-9) and dashes(-).
String S is non-empty.
```
```c++
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
        string res = "";
        for (int i = S.size() - 1; i >= 0; i--) {
            if (S[i] != '-') {
                ((res.size() % (K + 1) - K) ? res : res += '-') += toupper(S[i]);
            }
        }
        return string(res.rbegin(), res.rend());
    }
};

```
```c++
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
        int i = S.size()-1, curCount = 0;
        string R;
        
        while (i >= 0) {
            if (curCount == K) {
                R.push_back('-');
                curCount = 0;
            }
            
            if (S[i] != '-') {
                R.push_back(islower(S[i]) ? toupper(S[i]) : S[i]);
                curCount++;
            }
            
            i--;
        }
        
        if (R[R.size()-1] == '-')
            R.resize(R.size()-1);
        reverse(R.begin(), R.end());
        
        return R;
    }
};
```
```c++
class Solution {
public:
    string licenseKeyFormatting(string s, int k) {
        
        transform(s.begin(), s.end(), s.begin(), ::toupper); 
        string ans="";
        
        for(int i=0;i<s.length();i++){
            if(s[i]=='-')continue;
            else{
                ans+=s[i];
            }
        }
        
        if(ans.length()==0)return "";
        
        int count=0;
        int i=ans.length()-1;
        
        string ret;
       while(i>=0){
            
            while(i>=0 && count<k){
                ret+=ans[i];
                i--;
                count++;
            }
            ret+='-';
           count=0;
            
        }
        ret.pop_back();
        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```
```c++
class Solution {
public:
    string licenseKeyFormatting(string S, int K) {
		string res;
		for (int i = S.size()-1; i >= 0; --i){
			if (S[i] != '-'){
				if (res.size() % (K+1) == K) res += '-';
				res += toupper(S[i]);
			}
		}        
		reverse(res.begin(), res.end());
		return res;
    }
};
```