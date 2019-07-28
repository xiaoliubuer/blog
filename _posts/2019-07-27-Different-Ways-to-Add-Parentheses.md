---
layout: post
title:  "241. Different Ways to Add Parentheses"
date: 2019-07-27 20:04:00 -0400
categories: articles
---

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and  * .

Example 1:
```
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```
Example 2:
```
Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```
```c++
class Solution {
public:

    vector<int> diffWaysToCompute(string input) {
        vector<int> ans;
        bool pureNum = true;
        for ( int i = 0; i < input.length(); i++ ) 
            if (input[i]<'0' || input[i]>'9') {
            pureNum = false;
            vector<int> L=diffWaysToCompute(input.substr(0, i)), R=diffWaysToCompute(input.substr(i+1, input.length()-i-1));
                for (auto l : L)
                    for (auto r : R)
                        if (input[i]=='+') ans.push_back(l+r);
                        else if (input[i]=='-') ans.push_back(l-r);
                        else if (input[i]=='*') ans.push_back(l*r);
            }

        if (pureNum)
            ans.push_back(atoi(input.c_str()));
        return ans;
    }
};
```