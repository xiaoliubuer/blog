---
layout: post
title:  "321. Create Maximum Number"
date: 2019-07-30 19:33:00 -0400
categories: articles
---
Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k <= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits.

Note: You should try to optimize your time and space complexity.

Example 1:
```
Input:
nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
Output:
[9, 8, 6, 5, 3]
```
Example 2:
```
Input:
nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
Output:
[6, 7, 6, 0, 4]
```
Example 3:
```
Input:
nums1 = [3, 9]
nums2 = [8, 9]
k = 3
Output:
[9, 8, 9]
```

```c++
class Solution {
private:
    bool  greater(vector<int>& nums1,vector<int>& nums2,int i,int j){
        int m=nums1.size(),n=nums2.size();
        while(i<m && j<n && nums1[i]==nums2[j]){
            i++;
            j++;
        }
        return j==n || i<m && nums1[i]>nums2[j];
    }
    
    vector<int>   merger(vector<int> nums1,vector<int> nums2,int k){
        int m=nums1.size();
        int n=nums2.size();
        vector<int>  ans(k);
        for(int i=0,j=0,r=0;r<k;r++){
            ans[r]=greater(nums1,nums2,i,j)?nums1[i++]:nums2[j++];
        }
        return ans;
    }
    
    vector<int>   genMax(vector<int>& nums,int k){
        int m=nums.size();
        vector<int> ans(k);
        for(int i=0,j=0;i<m;i++){
            while(k-j<m-i && j>0 && ans[j-1]<nums[i]){
                j--;
            }
            if(j<k){
                ans[j++]=nums[i];
            }
        }
        return ans;
    }
    
public:
    vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
        int m=nums1.size(),n=nums2.size();
        vector<int>  ans(0);
        for(int i=max(0,k-n);i<=m && i<=k;i++){
            vector<int> tmp=merger(genMax(nums1,i),genMax(nums2,k-i),k);
            ans=greater(ans,tmp,0,0)? ans:tmp;
        }
        
        return ans;
    }
};
```
```c++
vector<int> maxNumber(vector<int>& nums1, vector<int>& nums2, int k) {
    int n1 = nums1.size(), n2 = nums2.size();
    vector<int> best;
    for (int k1=max(k-n2, 0); k1<=min(k, n1); ++k1)
        best = max(best, maxNumber(maxNumber(nums1, k1),
                                   maxNumber(nums2, k-k1)));
    return best;
}

vector<int> maxNumber(vector<int> nums, int k) {
    int drop = nums.size() - k;
    vector<int> out;
    for (int num : nums) {
        while (drop && out.size() && out.back() < num) {
            out.pop_back();
            drop--;
        }
        out.push_back(num);
    }
    out.resize(k);
    return out;
}

vector<int> maxNumber(vector<int> nums1, vector<int> nums2) {
    vector<int> out;
    while (nums1.size() + nums2.size()) {
        vector<int>& now = nums1 > nums2 ? nums1 : nums2;
        out.push_back(now[0]);
        now.erase(now.begin());
    }
    return out;
}
```