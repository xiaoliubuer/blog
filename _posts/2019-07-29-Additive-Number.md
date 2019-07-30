---
layout: post
title:  "306. Additive Number"
date: 2019-07-29 19:57:00 -0400
categories: articles
---

Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain at least three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits '0'-'9', write a function to determine if it's an additive number.

Note: Numbers in the additive sequence cannot have leading zeros, so sequence 1, 2, 03 or 1, 02, 3 is invalid.

Example 1:
```
Input: "112358"
Output: true 
Explanation: The digits can form an additive sequence: 1, 1, 2, 3, 5, 8. 
             1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```
Example 2:
```
Input: "199100199"
Output: true 
Explanation: The additive sequence is: 1, 99, 100, 199. 
             1 + 99 = 100, 99 + 100 = 199
```
Follow up:
```
How would you handle overflow for very large input integers?
```
```c++
class Solution {
public:
bool isAdditiveNumber(string num, string last1, string last2) {
	if (num[0] == '0' && num.length() > 1 || last1[0] == '0' && last1.length() > 1 || last2[0] == '0' && last2.length() > 1)
		return false;

	long long sum = stoll(last1) + stoll(last2);
	int length = (int)log10(sum) + 1;
	if (sum != stoll(num.substr(0, (int)log10(sum) + 1)))
		return false;
	if (num.substr(length).length() == 0)
		return true;

	return isAdditiveNumber(num.substr(length), last2, num.substr(0, length));
}

bool isAdditiveNumber(string num) {
	for (int i = 1; i<num.length(); i++) {
		for (int j = 1; j<num.length(); j++) {
			if (i + j >= num.length()) continue;
			if (isAdditiveNumber(num.substr(i + j), num.substr(0, i), num.substr(i, j)))
				return true;
		}
	}

	return false;
}
};
```