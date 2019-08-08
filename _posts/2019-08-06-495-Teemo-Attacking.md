---
layout: post
title:  "495. Teemo Attacking"
date: 2019-08-06 21:08:00 -0400
categories: articles
---
In LOL world, there is a hero called Teemo and his attacking can make his enemy Ashe be in poisoned condition. Now, given the Teemo's attacking ascending time series towards Ashe and the poisoning time duration per Teemo's attacking, you need to output the total time that Ashe is in poisoned condition.

You may assume that Teemo attacks at the very beginning of a specific time point, and makes Ashe be in poisoned condition immediately.

Example 1:
```
Input: [1,4], 2
Output: 4
Explanation: At time point 1, Teemo starts attacking Ashe and makes Ashe be poisoned immediately. 
This poisoned status will last 2 seconds until the end of time point 2. 
And at time point 4, Teemo attacks Ashe again, and causes Ashe to be in poisoned status for another 2 seconds. 
So you finally need to output 4.
```
Example 2:
```
Input: [1,2], 2
Output: 3
Explanation: At time point 1, Teemo starts attacking Ashe and makes Ashe be poisoned. 
This poisoned status will last 2 seconds until the end of time point 2. 
However, at the beginning of time point 2, Teemo attacks Ashe again who is already in poisoned status. 
Since the poisoned status won't add up together, though the second poisoning attack will still work at time point 2, it will stop at the end of time point 3. 
So you finally need to output 3.
```
Note:
```
You may assume the length of given time series array won't exceed 10000.
You may assume the numbers in the Teemo's attacking time series and his poisoning time duration per attacking are non-negative integers, which won't exceed 10,000,000.
```
```c++
class Solution {
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        if(!timeSeries.size()) return 0;
        
        int sum = 0;
        for(int i = 0; i < timeSeries.size() - 1; i++) {
            if(timeSeries[i] + duration > timeSeries[i+1]) {
                sum += (timeSeries[i+1] - timeSeries[i]); // ith will only contribute till (i+1) begins
            }
            else {
                sum += duration; // ith poison will last for duration
            }
        }
        sum += duration; // for the last activity
        
        return sum;
    }
};
```
```c++
class Solution {
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        // array & greedy
        // time complexity: O(n), space complexity: O(1)
        // 56ms, beats 94.32%
        
        // Main idea:
        // Use last index to record last attack time, and at each attack time add
        // need added duration before current time stamp
        int n = timeSeries.size(), res = 0;
        if(n < 1) { return res; }
        for(int i = 1; i < n; i++) {
            res += min(timeSeries[i] - timeSeries[i - 1], duration);
        }
        res += duration;
        return res;
    }
};
```
```c++
class Solution {
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        int res = 0;
        if(timeSeries.size() == 0) return res;
        for(int i = 0; i < timeSeries.size()-1; i++){
            //cout<<"Inside";
            res += min(duration, timeSeries[i+1] - timeSeries[i]);
        }
        res += duration;
        return res;
    }
};
```