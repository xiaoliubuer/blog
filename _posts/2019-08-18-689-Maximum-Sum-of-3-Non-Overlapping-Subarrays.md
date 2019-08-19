---
layout: post
title:  "689. Maximum Sum of 3 Non-Overlapping Subarrays"
date: 2019-08-18 17:57:00 -0400
categories: articles
---
In a given array nums of positive integers, find three non-overlapping subarrays with maximum sum.

Each subarray will be of size k, and we want to maximize the sum of all 3 * k entries.

Return the result as a list of indices representing the starting position of each interval (0-indexed). If there are multiple answers, return the lexicographically smallest one.

Example:
```
Input: [1,2,1,2,6,7,5,1], 2
Output: [0, 3, 5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
```
Note:
```
nums.length will be between 1 and 20000.
nums[i] will be between 1 and 65535.
k will be between 1 and floor(nums.length / 3).
```
```c++
class Solution {
public:
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int K) {
        int N = nums.size();
        vector<int> sums;
        vector<int> left(N);
        vector<int> right(N);
        
        int cur_sum = 0;
        for (int i = 0; i < K-1; ++i) cur_sum += nums[i];
        for (int i = K-1; i < N; ++i) {
            cur_sum += nums[i];
            sums.push_back(cur_sum);
            cur_sum -= nums[i-K+1];
        }
        
        int max_v = 0;
        int max_i = 0;
        
        for (int i = 0; i < sums.size(); ++i) {
            if (sums[i] > max_v) {
                max_v = sums[i];
                max_i = i;
            }
            left[i] = max_i;
        }
        
        max_v = 0;
        max_i = sums.size() - 1;
        for (int i = sums.size() - 1; i >= 0; --i) {
            if (sums[i] >= max_v) {
                max_v = sums[i];
                max_i = i;
            }
            right[i] = max_i;
        }
        
        int max_sum = 0;
        int i1, i2, i3;
        for (int i = K; i+K < sums.size(); ++i) {
            int cur_sum = sums[left[i-K]] + sums[i] + sums[right[i+K]];
            if (cur_sum > max_sum) {
                max_sum = cur_sum;
                i1 = left[i-K];
                i2 = i;
                i3 = right[i+K];
            }
        }
        
        return {i1, i2, i3};
    }
};
```
```c++
class Solution {
public:
    vector<int> maxSumOfThreeSubarrays(vector<int>& a, int k) {
        int n = a.size();
        vector<int> c[3], m(3); //store optimal solutions for 1 sum, 2 sums, 3 sums.
        vector<int> b(n);
        int sm = 0;
        for (int i = 0; i < n; ++i) {
            sm += a[i];
            if (i >=k-1) {
                b[i] = sm;
                sm -= a[i-k+1];
                if (i >= 3 * k-1) {
                    if (b[i-k-k] > m[0]) { // update 1 sum solution
                        m[0] = b[i-k-k];
                        c[0] = {i-k-k};
                    }
                    if (b[i-k] + m[0] > m[1]) { // update 2 sums solution
                        m[1] = b[i-k] + m[0];
                        c[1] = {c[0][0], i-k};
                    }
                    if (b[i] + m[1] > m[2]) { //update 3 sums solution
                        m[2] = m[1] + b[i];
                        c[2] = {c[1][0], c[1][1], i};
                    }
                }
            }
        }
        
        return {c[2][0]-k + 1,c[2][1]-k+1,c[2][2]-k+1};
    }
};
```

```c++
class Solution {
public:
    vector<int> prefix;
    int K = 0;
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int K) {
        int size = nums.size();
        this->K = K;
        prefix.resize(size);
        vector<int> suffix(size);
        vector<int> prev(size, -1);
        vector<int> back(size, -1);
        int sum = 0;
        int sum2 = 0;
        for(int i = 0; i < nums.size(); ++i) {
            prefix[i] = nums[i] + (i > 0 ? prefix[i-1] : 0);
            if(i + 1 >= K) {
                int kSum = getPrefixSum(i-K+1);
                if(sum < kSum) {
                    sum = kSum;
                    prev[i] = i;
                }else{
                    prev[i] = prev[i-1];
                }
            }
            int j = size - 1 - i;
            
            suffix[j] = nums[j] + (j == size-1 ? 0 : suffix[j+1]);
            if(size - j >= K) {
                int kSum = suffix[j] - (j+K == size ? 0 :suffix[j+K]);
                if(sum2 <= kSum) {
                    sum2 = kSum;
                    back[j] = j;
                }else{
                    back[j] = back[j+1];
                }
            }
        }
        vector<int> items;
        sum = 0;
        for(int i = K; i < size-K-K+1;++i) {
            int res = getPrefixSum(prev[i-1] - K + 1) + getPrefixSum(i) + getPrefixSum(back[i + K]);
            if(res > sum) {
                sum = res;
                items = { prev[i-1] - K + 1, i, back[i + K] };
            }
        }
        return items;
    }
    int getPrefixSum(int begin) {
        return prefix[begin + K - 1] - (begin > 0 ? prefix[begin-1] : 0);
    }
};
```
```c++
class Solution {
public:
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) {
        vector< int > cumSum{ 0 };
        int sum = 0;
        //Note: cumSum[ i ] - cumSum[ j ] sum from nums[ j ] to nums[ i - 1 ]
        for( int i = 0; i < nums.size(); i++ ){ 
            sum += nums[ i ];
            cumSum.push_back( sum );
        }
        //Note: preMax[ i ] means including nums[ i ], the max consecutive k sum;
        vector< int > preMax( nums.size(), 0 );
        vector< int > preMaxIdx( nums.size(), 0 );
        int preM = 0;
        for( int i = k - 1; i < nums.size(); i++ ){ 
            int sum = cumSum[ i + 1 ] - cumSum[ i + 1 - k ];
            //Note; > instead of >=
            if( sum > preM ){ 
                preMaxIdx[ i ] = i + 1 - k;
                preMax[ i ] = sum;
                preM = sum;
            }
            else{ 
                preMaxIdx[ i ] = preMaxIdx[ i - 1 ];
                preMax[ i ] = preM;
            }
        }
        vector< int > ans;
        int maxSum = 0;
        int postM = cumSum[ nums.size() ] - cumSum[ nums.size() - k ];
        int postMIdx = nums.size() - k;
        //Note: i is the start of postM, and nums i - k to i - 1 is the mid; i - k - 1 is preMax
        //Thus i range from 2k to nums.size() - k
        for( int i = nums.size() - k; i >= 2 * k; i-- ){ 
            //Note: if == we still move the postMIdx as left as possible.
            if( cumSum[ i + k ] - cumSum[ i ] >= postM ){ 
                postM = cumSum[ i + k ] - cumSum[ i ];
                postMIdx = i;
            }
            int tmp = cumSum[ i ] - cumSum[ i - k ] + postM + preMax[ i - k - 1 ]; 
            //Note: < instead of <=
            if( maxSum <= tmp ){ 
                ans = { preMaxIdx[ i - k - 1 ], i - k, postMIdx };
                maxSum = tmp;
            }
        }
        cout << "maxSum is " << maxSum << endl;
        return ans;
    }
};
```