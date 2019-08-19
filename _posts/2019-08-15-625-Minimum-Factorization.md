---
layout: post
title:  "625. Minimum Factorization"
date: 2019-08-15 08:39:00 -0400
categories: articles
---	
Given a positive integer a, find the smallest positive integer b whose multiplication of each digit equals to a.

If there is no answer or the answer is not fit in 32-bit signed integer, then return 0.

Example 1
```
Input:

48 
Output:
68
```
Example 2
```
Input:

15
Output:
35
```
```c++
class Solution {
    /*
        multiplication of each digit equals to a. 
        
        -> digits are factors of a => a % digit == 0 
            
        9 8 7 ... 1 
        
        32-bit signed 
        
        1. find all digits. 
        
        2. form the number 
        
    */
public:
    int smallestFactorization(int a) {
        if (a <= 1){
            return a; 
        }
        vector<int> digits;  
        for (int i = 9; i > 1 && a > 1 && digits.size() < 32; ){
            if (a % i == 0){
                a /= i; 
                digits.push_back(i); 
            }else{
                --i;
            }
        }
        if (a > 1){
            return 0; 
        }
        long number = 0; 
        for (int i = digits.size() - 1; i >= 0; --i){
            number = 10 * number + digits[i]; 
            if (number > std::numeric_limits<int>::max()){
                return 0; 
            }
        }
        return number; 
    }
};
```

```c++
class Solution {
public:
    int smallestFactorization(int n) {
	if (n == 1) return 1;
	long long m = 0, l = 1;
        for (int i = 9; i >= 2;)
        if (n % i == 0)
        {
            m += i*l;
            l *= 10;
            n /= i;
            if (m > INT_MAX)
                return 0;
        }else
            i --;
    return n == 1 ? m : 0;
    }
};
```
```c++
class Solution {
public:
int smallestFactorization(int a) {
        unsigned long long int ans=0,m=1;
        int i;
        if(a<10)
            return a;
        //factorisation
        for(i=9;i>=2;i--){
            while(a%i==0){
                a/=i;
                ans=m*i+ans;
                m*=10;
            }
        }
        //a!=1 checked to see if answer was even possible
        if(ans>INT_MAX || a!=1)
            return 0;
        return ans;
    }
};
```
