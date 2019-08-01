---
layout: post
title:  "397. Integer Replacement"
date: 2019-07-31 22:06:00 -0400
categories: articles
---
Given a positive integer n and you can do operations as follow:

If n is even, replace n with n/2.
If n is odd, you can replace n with either n + 1 or n - 1.
What is the minimum number of replacements needed for n to become 1?

Example 1:
```
Input:
8

Output:
3
```
Explanation:
8 -> 4 -> 2 -> 1

Example 2:
```
Input:
7

Output:
4
```
Explanation:
7 -> 8 -> 4 -> 2 -> 1
or
7 -> 6 -> 3 -> 2 -> 1
```c++
class Solution {
private:
    unordered_map<int, int> visited;

public:
    int integerReplacement(int n) {        
        if (n == 1) { return 0; }
        if (visited.count(n) == 0) {
            if (n & 1 == 1) {
                visited[n] = 2 + min(integerReplacement(n / 2), integerReplacement(n / 2 + 1));
            } else {
                visited[n] = 1 + integerReplacement(n / 2);
            }
        }
        return visited[n];
    }
};
```
```c++
class Solution {
public:
	int integerReplacement(long n) {
		int count = 0;
		while (n != 1) {
			// If n is even, replace n with n / 2.
			if (n % 2 == 0)
				n = n / 2;
			// If n is odd and if 1st bit position (NOT 0th position) is NOT 1 then subtract 1
			// Check for n==3 is corner case
			else if ((n == 3) || ((n & (1 << 1)) == 0))
				n = n - 1;
			else
				n = n + 1;
			count++;
		}
		return count;
	}
};
```
```c++
class Solution {
public:
    
    void check(long n,int ope,int &res)
    {
        if(n==1)
        {
            res=min(res,ope);
            return ;
        }
        
        if(n&1)
        {
            check(n-1,ope+1,res);
            check(n+1,ope+1,res);
        }
        else
        {
            check(n>>1,ope+1,res);
        }
    }
    
    int integerReplacement(int n) {
        int res=INT_MAX;
        check(n,0,res);
        return res;
    }
};
```