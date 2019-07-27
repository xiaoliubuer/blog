---
layout: post
title:  "908. Smallest Range I"
date: 2019-07-26 08:03:00 -0400
categories: articles
---

Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].

After this process, we have some array B.

Return the smallest possible difference between the maximum value of B and the minimum value of B.

Example 1:
```
Input: A = [1], K = 0
Output: 0
Explanation: B = [1]
```
Example 2:
```
Input: A = [0,10], K = 2
Output: 6
Explanation: B = [2,8]
```
Example 3:
```
Input: A = [1,3,6], K = 3
Output: 0
Explanation: B = [3,3,3] or B = [4,4,4]
```
Note:
```
1 <= A.length <= 10000
0 <= A[i] <= 10000
0 <= K <= 10000
```
```c++
class Solution {
public:
    int smallestRangeI(vector<int>& A, int K) {
        int mx = A[0], mn = A[0];
        for ( auto i : A ) {
            mx = max(mx, i);
            mn = min(mn, i);
        }
        return max(0, (mx - K) -  (mn + K));
    }
};
```
```c++
class Solution {
public:
    int smallestRangeI(vector<int> A, int K) {
        int mx = A[0], mn = A[0];
        for (int a : A) mx = max(mx, a), mn = min(mn, a);
        return max(0, mx - mn - 2 * K);
    }
};
```
Too understand this answer:

Suppose the input is [1, 5, 6], k = 3. We should only consider the min and max number in the array. Why? Because suppose the different between 5 and 6. The question reqire us must +k or -k for each element. So, for 5, must +3 or -3, same 6 must +3 or -3. If 5 and 6 all +3 or all -3. There's nothing changed. So in between of them, should one +3, another one -3. Which one + and which one -? Obviously, 5 + 3 and 6 - 3 is a better way, the gap will be 8 - 3 = 5, otherwire if 5 -3, 6 + 3, gap = 9 - 2 = 7. Thus, in a string, the most changes should happens in the max value and min value. In that way, we should only consider the changes between max and min for min + k and max - k.