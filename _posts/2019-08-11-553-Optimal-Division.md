---
layout: post
title:  "553. Optimal Division"
date: 2019-08-11 16:10:00 -0400
categories: articles
---

Given a list of positive integers, the adjacent integers will perform the float division. For example, [2,3,4] -> 2 / 3 / 4.

However, you can add any number of parenthesis at any position to change the priority of operations. You should find out how to add parenthesis to get the maximum result, and return the corresponding expression in string format. Your expression should NOT contain redundant parenthesis.

Example:
```
Input: [1000,100,10,2]
Output: "1000/(100/10/2)"
Explanation:
1000/(100/10/2) = 1000/((100/10)/2) = 200
However, the bold parenthesis in "1000/((100/10)/2)" are redundant, 
since they don't influence the operation priority. So you should return "1000/(100/10/2)". 
```
Other cases:
```
1000/(100/10)/2 = 50
1000/(100/(10/2)) = 50
1000/100/10/2 = 0.5
1000/100/(10/2) = 2
```
Note:
```
The length of the input array is [1, 10].
Elements in the given array will be in range [2, 1000].
There is only one optimal division for each test case.
```
```c++
class Solution {
public:
    string optimalDivision(vector<int>& nums) {
        
        if(nums.size()==1)
            return to_string(nums[0])+"";
        if(nums.size()==2)
            return to_string(nums[0])+"/"+to_string(nums[1]);
        string res=to_string(nums[0])+"/("+to_string(nums[1]);
        for(int i=2;i<nums.size();i++)
            res=res+"/"+to_string(nums[i]);
        res=res+")";
        return res;
        
    }
};
```
```c++
class Solution {
public:
    string optimalDivision(vector<int>& nums) {
        std::string s;
        s += std::to_string(nums[0]);
        if (nums.size() == 1) {
            return s;
        }
        
        s += "/";
        if (nums.size() == 2) {
            s += std::to_string(nums[1]);
            return s;
        }
        
        s += "(";
        for (size_t ii = 1; ii < nums.size(); ii++) {
            s += std::to_string(nums[ii]) + "/";
        }

        s.pop_back();
        s += ")";
        
        return s;
    }
};
```
```c++
class Solution {
public:
    string optimalDivision(vector<int>& nums) {
        //Answer is always 1000 / ( continuesly divident....)
        
        string result = "";
        if(nums.size() == 0) return result;
        if(nums.size() == 1) return to_string(nums[0]);
        if(nums.size() == 2) {
            result = to_string(nums[0]) + "/" + to_string(nums[1]);
            return result;
        }
        
        
        result = result + to_string(nums[0]);
        result = result + "/(";
        result = result + to_string(nums[1]);

        for(int i = 2; i < nums.size(); ++i) {
            result = result + '/' + to_string(nums[i]);
        }
        result = result + ')';
        return result;
    }
};
```