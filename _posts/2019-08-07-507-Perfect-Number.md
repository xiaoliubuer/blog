---
layout: post
title:  "507. Perfect Number"
date: 2019-08-07 19:20:00 -0400
categories: articles
---
We define the Perfect Number is a positive integer that is equal to the sum of all its positive divisors except itself.

Now, given an integer n, write a function that returns true when it is a perfect number and false when it is not.
Example:
```
Input: 28
Output: True
Explanation: 28 = 1 + 2 + 4 + 7 + 14
```
Note: The input number n will not exceed 100,000,000. (1e8)
```c++
class Solution {
public:
    bool checkPerfectNumber(int num) {
        if (num == 0 || num == 1) return false;
        int sum = 1;
        int div = 2;
        int ans;
        while ((ans = num/div) >= div) {
            if (num % div == 0) {
                if (ans != div) sum += (div + ans);
                else sum += div;
            }
            div++;
        }
        return sum == num;
    }
};
```
```c++
class Solution {
public:
    bool checkPerfectNumber(int num) {
        if (num <= 1)
            return false;

        int sum = 1;
        int high = (int)sqrt(num);
        for (int d = 2; d <= high; ++d) {
            if (num %d == 0) {
                sum += d + num/d;
            }
        }

        return sum == num;
    }
};
```

```c++
class Solution {
public:
bool checkPerfectNumber(int num) {
if (num <2) return false;
int temp = num;
int sum=1;
int i = 2;

	while (temp % 2 == 0) {
		temp /= 2;
		sum += (temp + i);
		i *=2;
	}

	return (num == sum);
}
};
```