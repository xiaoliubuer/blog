---
layout: post
title:  "* 299. Bulls and Cows"
date: 2019-01-01 13:35:23 -0400
categories: articles
---
You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.

Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows. 

Please note that both secret number and friend's guess may contain duplicate digits.

Example 1:
```
Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.
```
Example 2:
```
Input: secret = "1123", guess = "0111"

Output: "1A1B"

Explanation: The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.
```
Note: You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.
# 题意
两个string of number，一个是原来的，另一个是猜的，位置和数字都对，“bulls“，只有数字对，”cows“。

# Funciton signature
```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        
    }
};
```
# 想法
用一个map，把所有的数组扫一遍保存起来
然后从头到位遍历string1 和stirng2，如果当前位置数字相同，A++, map[c]--, 否则到map中找，如果>0, map[c]--, B++
最后按照A和B的值生成A和B的string

# 错误答案
```c++
class Solution {
public:
    string getHint(string secret, string guess) {
    	unordered_map<char, int> mark;
    	int A = 0;
    	int B = 0;
    	for (int i = 0; i < secret.size() && i < guess.size(); i++){
    		if (secret[i] == guess[i]) {
    			A++;
    			if (mark[secret] > 0 ) {
    				mark[secret[i]]--;
    				B--;
    			}

    		}
    		else{
    			if (mark.find(guess[i]) != mark.end()){
    				B++;
    				mark[guess[i]];
    			}
    		}
    	}
       return to_string(A) + "A" + to_string(B) + "B";
    }
};
// input
// "1122"
// "1222"
// Output
// "3A1B"
// Expected
// "3A0B"
```
# 看答案
```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        int bull=0;
        int cow =0;
        vector<int> ba(10,0);
        vector<int> ca(10,0);
        for(int i =0;i<secret.size();i++){
            if(secret[i]==guess[i]){
                bull++;
            }
            else{
                ba[secret[i]-'0']++;
                ca[guess[i]-'0']++;
            }
        }
        for(int i=0;i<10;i++){
            cow += min(ba[i],ca[i]);
        }
        return to_string(bull)+'A'+to_string(cow)+'B';
    }
};
```