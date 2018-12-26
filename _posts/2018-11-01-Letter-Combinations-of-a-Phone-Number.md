---
layout: post
title:  "* 17. Letter Combinations of a Phone Number"
date: 2018-11-01 07:34:23 -0400
categories: articles
---

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.



Example:

Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.
# 题意：
就是个一个手机键盘，给一个digits string，返回所有的数字的组合的可能性。
比如：
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].


# 思路：
就是把得到的每个键上的每个字母都忘原来的键上加一个现有的

怎么说呢？

比如：
输入的字母是： 23
那么也就是：	“abc” ，”def“
```
a  //Outside loop
---
	ad // Inside loop
	ae
	af
b 
---
	bd
	be
	bf
c
---
	cd
	ce
	cf
```
## ccccc！！！脑子还是没想清楚！！！！根本不是编程能力的问题！！就是脑子里面一片浆糊！！！
```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> keyboard = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> res;
        if (digits.size() == 0) return res;
        res.push_back("");
        for (int i = 0; i < digits.size(); i++){
            int idx = digits[i] - '0';
            string key = keyboard[idx];
            vector<string> temp;
            for (int j = 0; j < key.size(); j++){
                for (int k = 0; k < res.size(); k++){
                    temp.push_back(res[k] + key[j]);
                }
            }
            res = temp;
        }
        return res;
    }
};
```

用递归的DFS，那么就是loop digits，然后把idx，digits，以及res，还有keyboard传入helper函数

传进去以后，遍历key？还是遍历剩下的digit？

## DFS这里需要继续考虑！！！！

```c++
class Solution {
public:
    vector<string> result;
    unordered_map<int,string> mp;
    vector<string> letterCombinations(string digits) {
    	vector<string> mp = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        if(digits.size()==0)
            return result;
        string cur = "";
        dfs(0, cur, digits);
        return result;
    }
    void dfs(int index, string cur, string &digits){
        if(index == digits.size()){
            result.push_back(cur);
            return;
        }
        string options = mp[digits[index]-'0'];
        for(int i = 0; i < options.size(); ++i){
            dfs(index + 1, cur + options[i], digits);
        }
    }
};
```
