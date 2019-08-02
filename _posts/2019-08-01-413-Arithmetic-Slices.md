---
layout: post
title:  "413. Arithmetic Slices"
date: 2019-08-01 20:47:00 -0400
categories: articles
---
A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequence:
```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
The following sequence is not arithmetic.

1, 1, 2, 5, 7
```
A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of array A is called arithmetic if the sequence:
A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.

The function should return the number of arithmetic slices in the array A.


Example:
```
A = [1, 2, 3, 4]

return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.
```
```c++

class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for (int x : nums) {
            sum += x;
        }
        if (sum % 2 != 0) return false;
        sort(nums.begin(), nums.end(), greater<int> ());
        vector<int> subset(2, 0);
        return DFSHelper(nums, subset, sum / 2, 0);
    }
    
    bool DFSHelper(vector<int>& nums, vector<int>& subset, int target, int index) {
        if (index == nums.size()) return subset[0] == subset[1];
        
        for (int i = 0; i < 2; i++) {
            if (i > 0 && subset[i] == subset[i - 1]) continue;
            if (subset[i] + nums[index] > target) continue;
            subset[i] += nums[index];
            if (DFSHelper(nums, subset, target, index + 1)) return true;
            subset[i] -= nums[index];
        }
        return false;
    }
};
```
```c++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
		int count = 0;
		for(int i = 0; i < A.size(); ++i){
			for(int j = i + 2; j < A.size(); ++j){
				if(A[j] - A[j - 1] == A[j - 1] - A[j - 2]){
					++count;
				}
				else{
					break;
				}
			}
		}
		return count;
    }
};
```
```c++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
        if (A.size()<3) return 0; // check if the array greater than 3
        vector<int>B(A.size(),0); //using the memorize how many consectutive number with same gap in the list
        int gap = A[1] - A[0]; // the first gap
        int result = 0;
        for (int i = 2; i< A.size(); i++){
            if (A[i]-A[i-1] == gap){ // if the gap equal to the previous one, plus one 
                B[i] = B[i-1] + 1;
                result += B[i]; 
            }
            gap = A[i]-A[i-1];
        }
        return result;
        
        
    }
};
```