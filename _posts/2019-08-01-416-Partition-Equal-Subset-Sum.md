---
layout: post
title:  "416. Partition Equal Subset Sum"
date: 2019-08-01 20:57:00 -0400
categories: articles
---
Given a non-empty array containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

Note:

Each of the array element will not exceed 100.
The array size will not exceed 200.
 

Example 1:
```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

Example 2:
```
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```
```c++
class Solution {
public:
bool canPartition(vector<int>& nums) {
    int n = nums.size();
    int sum = 0 ;
    vector<int> visited(n,0);
    vector<int> tailsum(n + 1,0);
	//record the sum of which is from the i-th number to the last number for Pruning
	//sort the array, and pick the bigger number firstly
    sort(nums.begin(),nums.end(),greater<int>());
    for(int i = n - 1 ; i >= 0 ; i --){
    	sum += nums[i];
        tailsum[i] = tailsum[i + 1] + nums[i];
    }
    if(sum & 1) return false;
    return dfs(nums,visited,tailsum,0,sum/2,0);
}
bool dfs(vector<int>& nums,vector<int>& visited,vector<int>& tailsum,int cur_sum,int target,int begin)
{
	if(cur_sum == target)
		return true;
	if(cur_sum > target)
		return false;
	//the sum of the rest is less than target
    if(cur_sum + tailsum[begin] < target)
        return false;
	for(int i = begin ; i <nums.size() ; i++)
	{
		if(visited[i] == 0)
		{
			visited[i] = 1;
			if(dfs(nums,visited,tailsum,cur_sum + nums[i],target,i+1))
				return true;
			visited[i] = 0;
		}
	}
	return false;
}
};
```