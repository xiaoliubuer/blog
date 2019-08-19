---
layout: post
title:  "575. Distribute Candies"
date: 2019-08-11 17:30:00 -0400
categories: articles
---
Given an integer array with even length, where different numbers in this array represent different kinds of candies. Each number means one candy of the corresponding kind. You need to distribute these candies equally in number to brother and sister. Return the maximum number of kinds of candies the sister could gain.
Example 1:
```
Input: candies = [1,1,2,2,3,3]
Output: 3
```
Explanation:
```
There are three different kinds of candies (1, 2 and 3), and two candies for each kind.
Optimal distribution: The sister has candies [1,2,3] and the brother has candies [1,2,3], too. 
The sister has three different kinds of candies. 
```
Example 2:
```
Input: candies = [1,1,2,3]
Output: 2
```
Explanation: 
```
For example, the sister has candies [2,3] and the brother has candies [1,1]. 
The sister has two different kinds of candies, the brother has only one kind of candies. 
```
Note:
```
The length of the given array is in range [2, 10,000], and will be even.
The number in given array is in range [-100,000, 100,000].
```
```c++
class Solution {
public:
    int distributeCandies(vector<int>& candies) {
        unordered_set<int> kinds;
        int candies_count_half = candies.size() / 2;
        
        for (auto candy : candies) {
            kinds.insert(candy);
            if (kinds.size() >= candies_count_half) return candies_count_half;
        }
        
        return kinds.size();
    }
};
```
```c++
class Solution {
public:
    int distributeCandies(vector<int>& candies) {
        unordered_set<int> st;
        for (auto c : candies) {
            st.insert(c);
        }
        return min(int(st.size()), int(candies.size() / 2));
    }
};
```
```c++
class Solution {
public:
    int distributeCandies(vector<int>& candies) {
        sort(candies.begin(), candies.end());
        int count = 1;
        for (int i = 1; i < candies.size() && count < candies.size() / 2; i++){
            if (candies[i] > candies[i-1]) count++;
        }
        return count;
    }
};
```
```c++
class Solution {
public:
    int distributeCandies(vector<int>& candies) {
        std::sort(candies.begin(),candies.end());
        int count = 1;
        for(int i = 0; i < candies.size() -1 && count < candies.size()/2; ++i)
        {
            if(candies[i+1] > candies[i])
                ++count;
        }
        return count;
        
    }
};
```
```c++
class Solution {
public:
    int distributeCandies(vector<int>& candies) {
        map<int,int> m;
        for(int i=0; i<candies.size(); i++){
            m[candies[i]]++;
        }
        int typesOfCandies = m.size();
        if(candies.size()/2<=typesOfCandies)
            return candies.size()/2;
        else
            return typesOfCandies;
    }
};
```