---
layout: post
title:  "91. Decode Ways"
date: 2019-06-24 19:33:00 -0400
categories: articles
---

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1:
```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```
Example 2:
```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```
```c++
class Solution {
public:
    int numDecodings(string s) {
        if(s.empty() || s[0]<'1' || s[0]>'9') return 0;
        vector<int> dp(s.size()+1,0);
        dp[0] = dp[1] = 1;
        
        for(int i = 1; i < s.size(); i++) {
            if( !isdigit(s[i]) ) return 0;
            int v = stoi(s.substr(i-1,2));
            if( v <= 26 && v > 9 ) dp[i+1] += dp[i-1];
            if( s[i] != '0' ) dp[i+1] += dp[i];
            if( dp[i+1] == 0 ) return 0;
        }
        return dp[s.size()];
    }
};
```
```c++
class Solution {
public:
    int numDecodings(string s) {
 		if(s.size() == 0) return 0;
 		vector<int> dp(s.size());
 		dp[-1] = 0;
 		if(s.size() == 1 && s[0] != '0')
 			return 1;

 		if ( s[0]-'0' == 0){
 			dp[0] = 0;
 			return 0;
 		}
 		else
 			dp[0] = 1;
 		

 		if( s[0]-'0' > 2 || s[0]-'0' == 2 && s[1]-'0' >6 ){
 			dp[1] = 1;
 		}
 		else{
 			dp[1] = 2;
 		}

 		if (s[1] - '0' == 0){
 			if(s[0] - '0' < 3 )
 				dp[1] = 1;
 			else
 				return 0;
 		}

 		for (int i = 2; i < s.size(); ++i){

 			if ( s[i]-'0' == 0 ){
 				if ( s[i-1]-'0' == 0 || s[i-1]-'0' > 2 ){
 					return 0;
 				}
 				else{
 					dp[i] = dp[i-2];
 					continue;
 				}
 			}
 			else {
 				if(s[i-1]-'0' > 2 || s[i-1]-'0' == 2 && s[i]-'0' >6 )
 					dp[i] = dp[i-1];
 				else{
	                if(s[i-1]-'0' == 0 )
	                    dp[i] = dp[i-1];
	                else
	 					dp[i] = dp[i-2] + dp[i-1];
            	}
        	}
 		}

 		return dp[s.size()-1];
    }

};
```