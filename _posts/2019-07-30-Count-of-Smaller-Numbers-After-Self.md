---
layout: post
title:  "315. Count of Smaller Numbers After Self"
date: 2019-07-30 08:43:00 -0400
categories: articles
---
You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

Example:
```
Input: [5,2,6,1]
Output: [2,1,1,0] 
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```
```c++
class Solution {
    vector<int>bit;
    int n;
public:
    void update(int index, int val){
        while(index<=n){
            bit[index]+=val;
            index+=(index&(-index));
        }
    }
    
    int getSum(int index){
        int sum=0;
        while(index>0){
            sum+=bit[index];
            index-=(index&(-index));
        }
        
        return sum;
    }
    
    vector<int> countSmaller(vector<int>& nums) {
        bit.resize(nums.size()+1,0);
        n=nums.size();
        
        convert(nums);
        vector<int> ans(n,0);
        for(int i=n-1;i>=0;i--){
            //cout<<"nums[i]="<<nums[i]<<"\n";
            ans[i]=getSum(nums[i]-1);
            update(nums[i],1);
        }
        
        return ans;
    }
    
    void convert(vector<int>& nums){
        map<int,int> h;
        for(int x:nums) h[x]=1;
        int index=0;
        for(auto it: h){
            h[it.first]=++index;
        }
        
        for(int i=0;i<n;i++){
            nums[i]=h[nums[i]];
        }
    }
};
```
```c++
class Solution {
protected:
    void merge_countSmaller(vector<int>& indices, int first, int last, 
                            vector<int>& results, vector<int>& nums) {
        int count = last - first;
        if (count > 1) {
            int step = count / 2;
            int mid = first + step;
            merge_countSmaller(indices, first, mid, results, nums);
            merge_countSmaller(indices, mid, last, results, nums);
            vector<int> tmp;
            tmp.reserve(count);
            int idx1 = first;
            int idx2 = mid;
            int semicount = 0;
            while ((idx1 < mid) || (idx2 < last)) {
                if ((idx2 == last) || ((idx1 < mid) &&
                       (nums[indices[idx1]] <= nums[indices[idx2]]))) {
					tmp.push_back(indices[idx1]);
                    results[indices[idx1]] += semicount;
                    ++idx1;
                } else {
					tmp.push_back(indices[idx2]);
                    ++semicount;
                    ++idx2;
                }
            }
            move(tmp.begin(), tmp.end(), indices.begin()+first);
        }
    }
public:
    vector<int> countSmaller(vector<int>& nums) {
        int n = nums.size();
        vector<int> results(n, 0);
        vector<int> indices(n, 0);
        iota(indices.begin(), indices.end(), 0);
        merge_countSmaller(indices, 0, n, results, nums);
        return results;
    }
};
```