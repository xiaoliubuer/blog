---
layout: post
title:  "477. Total Hamming Distance"
date: 2019-08-06 19:47:00 -0400
categories: articles
---
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Now your job is to find the total Hamming distance between all pairs of the given numbers.

Example:
```
Input: 4, 14, 2

Output: 6

Explanation: In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
showing the four bits relevant in this case). So the answer will be:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.
```
Note:
Elements of the given array are in the range of 0 to 10^9
Length of the array will not exceed 10^4.

```c++
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int dist = 0, size = nums.size();
        for(int i = 0; i < 32; ++i) {
            int count = 0, mask = 1 << i;
            for(const auto &n: nums)
                if(n & mask) ++count; // If mask not zero, we have a 1 bit
            dist += count * (size-count); // All other bits are zero, so size-count = 0 bits
        }
        return dist;
    }
};
```
```c++
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int result = 0;
        for(int t = 31; t >= 0; --t){
            int ones = 0;
            int zeros = 0;
            for(auto n : nums){
                if((n>>t)&1){
                    ++ones;
                }else{
                    ++zeros;
                }
            }
            result += ones*zeros;
        }
        return result;
    }
};
```