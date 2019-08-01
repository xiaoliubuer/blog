---
layout: post
title:  "360. Sort Transformed Array"
date: 2019-07-31 19:59:00 -0400
categories: articles
---
Given a sorted array of integers nums and integer values a, b and c. Apply a quadratic function of the form f(x) = ax2 + bx + c to each element x in the array.

The returned array must be in sorted order.

Expected time complexity: O(n)

Example 1:
```
Input: nums = [-4,-2,2,4], a = 1, b = 3, c = 5
Output: [3,9,15,33]
```
Example 2:
```
Input: nums = [-4,-2,2,4], a = -1, b = 3, c = 5
Output: [-23,-5,1,7]
```

```c++
class Solution {
    public:
        vector<int> sortTransformedArray(vector<int>& nums, int a, int b, int c) {
            vector<int> res(nums.size());
            if (nums.size() == 0) return res;
            int i = 0, j = nums.size() - 1;
            if (a > 0) {
                int index = nums.size() - 1;
                while (i <= j) {
                    if (transform(nums[i], a, b, c) > transform(nums[j], a, b, c)) {
                        res[index--] = transform(nums[i], a, b, c);
                        i++;
                    } else {
                        res[index--] = transform(nums[j], a, b, c);
                        j--;
                    }
                }
            } else {
                int index = 0;
                while (i <= j) {
                    if (transform(nums[i], a, b, c) < transform(nums[j], a, b, c)) {
                        res[index++] = transform(nums[i], a, b, c);
                        i++;
                    } else {
                        res[index++] = transform(nums[j], a, b, c);
                        j--;
                    }
                }
            }
            return res;
        }

        int transform(int num, int a, int b, int c) {
            return a * num * num + b * num + c;
        }
};
```