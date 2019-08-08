---
layout: post
title:  "506. Relative Ranks"
date: 2019-08-07 19:18:00 -0400
categories: articles
---
Given scores of N athletes, find their relative ranks and the people with the top three highest scores, who will be awarded medals: "Gold Medal", "Silver Medal" and "Bronze Medal".

Example 1:
```
Input: [5, 4, 3, 2, 1]
Output: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
Explanation: The first three athletes got the top three highest scores, so they got "Gold Medal", "Silver Medal" and "Bronze Medal". 
For the left two athletes, you just need to output their relative ranks according to their scores.
```
Note:
```
N is a positive integer and won't exceed 10,000.
All the scores of athletes are guaranteed to be unique.
```
```c++
bool customSort(const pair<int,int> &p1, const pair<int, int> &p2){
    return (p1.first > p2.first);
}

class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& nums) {
        vector<string> ans(nums.size());
        vector<pair<int, int>> store;
        for(int i=0; i<nums.size(); i++){
            store.push_back(make_pair(nums[i], i));
        }
        sort(store.begin(), store.end(), customSort);
        int i = 0;
        for(auto itr : store){
            if(i == 0) ans[itr.second] = "Gold Medal";
            else if(i == 1) ans[itr.second] = "Silver Medal";
            else if(i == 2) ans[itr.second] = "Bronze Medal";
            else ans[itr.second] = to_string(i+1);
            i++;
        }
        store.clear();
        return ans;
    }
};
```

```c++
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& nums) {
        vector<int> numsCopy(nums);
        vector<string> ans;
        sort(numsCopy.begin(), numsCopy.end(), greater<int>()); 
        for (int num : nums) {
            int pos = 0;
            int left = 0, right = numsCopy.size() - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (numsCopy[mid] == num) {
                    pos = mid;
                    break;
                }
                if (numsCopy[mid] < num) right = mid - 1;
                else left = mid + 1;
            }
            if (pos == 0) ans.push_back("Gold Medal");
            else if (pos == 1) ans.push_back("Silver Medal");
            else if (pos == 2) ans.push_back("Bronze Medal");
            else ans.push_back(to_string(pos + 1));
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& nums) {
        int n=nums.size(),i;
        vector<string>ans(n);
        map<int,int>mp;
        for(i=0;i<n;i++){
            mp[nums[i]]=i;
        }
        sort(nums.begin(),nums.end(),greater<int>());
        ans[mp[nums[0]]]="Gold Medal";
        if(n>1){
            ans[mp[nums[1]]]="Silver Medal";
        }
        if(n>2){
            ans[mp[nums[2]]]="Bronze Medal";
        }
        for(i=3;i<n;i++){
            ans[mp[nums[i]]]=to_string(i+1);
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
  vector<string> findRelativeRanks(vector<int>& nums) 
  {
    vector<int> numsCopy = nums;
    vector<string> op;
    sort(numsCopy.begin(), numsCopy.end(), greater <>());
    for (size_t i = 0; i < nums.size(); i++)
    {
      auto pos = find(numsCopy.begin(), numsCopy.end(), nums[i]);
      op.push_back(to_string(pos - numsCopy.begin() + 1));
    }
    if (nums.size() >= 1)
    {
      auto pos = find(nums.begin(), nums.end(), numsCopy[0]);
      op[pos - nums.begin()] = "Gold Medal";
    }
    if (nums.size() >= 2)
    {
      auto pos = find(nums.begin(), nums.end(), numsCopy[1]);
      op[pos - nums.begin()] = "Silver Medal";
    }
    if (nums.size() >= 3)
    {
      auto pos = find(nums.begin(), nums.end(), numsCopy[2]);
      op[pos - nums.begin()] = "Bronze Medal";
    }
    return op;
  }
};
```