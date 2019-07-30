---
layout: post
title:  "294. Flip Game II"
date: 2019-07-28 20:06:00 -0400
categories: articles
---
You are playing the following Flip Game with your friend: Given a string that contains only these two characters: + and -, you and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to determine if the starting player can guarantee a win.

Example:
```
Input: s = "++++"
Output: true 
Explanation: The starting player can guarantee a win by flipping the middle "++" to become "+--+".
```
Follow up:
Derive your algorithm's runtime complexity.

```c++
bool canWin(string s) {
        int len = s.size();
        for(int i=0;i<(len-1);i++){
            if(s[i]=='+' && s[i+1]=='+' ){
                s[i]= s[i+1] = '-';
                if( !canWin(s) )return true;//after flip, if B can win
                s[i]= s[i+1] = '+';
            }
        }
        return false;//either no available flip or no guarantee win move
    }
```
```c++
class Solution {
public:
	bool helper(string s){
		for (int i = 0; i+1 < s.size(); ++i){
			if ( s[i] == '+' && s[i+1] == '+'){
				s[i] = s[i+1] = '-';
				if(helper(s) == false) return true; //只要有一个情况player2可以成功，那么player1就会失败
				s[i] = s[i+1] = '+';
			}
		}
		return false;
	}
    bool canWin(string s) {
    	if (s.size() < 2 ) return false;
    	return helper(s);
    }
};
```
```c++
class Solution {
private:
    unordered_map<string, bool> dp;
public:
    bool canWin(string s) {
        if (dp.find(s) != dp.end())
            return dp[s];
        size_t pre = 0, found = 0;
        while((found = s.find("++", pre)) != string::npos) {
            if (!canWin(s.substr(0, found) + "--" + s.substr(found + 2)))
                return dp[s] = true;
            pre = found + 1;
        }
        return dp[s] = false;
    }
};
```