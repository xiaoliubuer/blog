---
layout: post
title:  "479. Largest Palindrome Product"
date: 2019-08-06 19:53:00 -0400
categories: articles
---
Find the largest palindrome made from the product of two n-digit numbers.

Since the result could be very large, you should return the largest palindrome mod 1337.
Example:
```
Input: 2

Output: 987
Explanation: 99 x 91 = 9009, 9009 % 1337 = 987
```
Note:

The range of n is [1,8].

```c++
class Solution {
public:
  int largestPalindrome(int n) {
	if (n == 1)return 9;
	long long max = pow(10, n) - 1;
	for (int v = max - 1; v>(max / 10); v--) {
		string s = to_string(v), s0 = s;
		reverse(s.begin(), s.end());
		long long u = atoll((s0 + s).c_str());
		for (long long x = max; x*x >= u; x--)if (u%x == 0)return(int)(u%1337);
	}
	return 0;
    }
};
```
```c++
class Solution {
public:
    int largestPalindrome(int n) {
        int upper = pow(10, n) - 1, lower = upper / 10;
        
        for (int i=upper; i>lower; i--) {
            string str = to_string(i);
            string rev = str;
            reverse(rev.begin(), rev.end());
            long long int res = stol(str + rev);
            
            for (long long int j=upper; j*j>res; j--) {
                if (res % j == 0)
                    return res % 1337;
            }   
        }
        
        return 9;
    }
};
```
```c++
class Solution {
public:
    int largestPalindrome(int n) {
        if(n==1) return 9;
        int h=pow(10,n)-1;
        int l=pow(10,n-1);
        for(int i=h;i>=l;i--){
            string t=to_string(i);
            string s=t;
            reverse(t.begin(),t.end());
            s+=t;
            long long  num=stoll(s);
           // cout<<num<<endl;
            for(long long j=h;j*j>num;j--){
                if(num%j==0) return num%1337;
                
            }
        }
        return -1;
    }
    
};
```
```c++

class Solution {
public:
     int largestPalindrome(int n) {
	     if (n == 1)return 9;
	     long long max = pow(10, n) - 1;
       for (int v = max - 1; v>(max / 10); v--) {
	       string s = to_string(v), s0 = s;
		     reverse(s.begin(), s.end());
		     long long u = atoll((s0 + s).c_str());
         // cout << u << endl;
		     for (long long i=max; (i*i)>u; i--){
           if (u%i==0) return u%1337;
         }
	     }
	     return 0;
    }
};
```