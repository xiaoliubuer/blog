---
layout: post
title:  "592. Fraction Addition and Subtraction"
date: 2019-08-11 18:07:00 -0400
categories: articles
---

Given a string representing an expression of fraction addition and subtraction, you need to return the calculation result in string format. The final result should be irreducible fraction. If your final result is an integer, say 2, you need to change it to the format of fraction that has denominator 1. So in this case, 2 should be converted to 2/1.

Example 1:
```
Input:"-1/2+1/2"
Output: "0/1"
```
Example 2:
```
Input:"-1/2+1/2+1/3"
Output: "1/3"
```
Example 3:
```
Input:"1/3-1/2"
Output: "-1/6"
```
Example 4:
```
Input:"5/3+1/3"
Output: "2/1"
```
Note:
```
The input string only contains '0' to '9', '/', '+' and '-'. So does the output.
Each fraction (input and output) has format ±numerator/denominator. If the first input fraction or the output is positive, then '+' will be omitted.
The input only contains valid irreducible fractions, where the numerator and denominator of each fraction will always be in the range [1,10]. If the denominator is 1, it means this fraction is actually an integer in a fraction format defined above.
The number of given fractions will be in the range [1,10].
The numerator and denominator of the final result are guaranteed to be valid and in the range of 32-bit int.
```
```c++
class Solution {
public:
    string fractionAddition(string expression) {
        string& s = expression; int size = s.size();
        int n = 0; int d = 1; 
        bool sign = false; int i = 0; 
        while(i<size) {
            if(s[i] == '-') {sign = true; i+=1;}
            else if(s[i] == '+') {sign = false; i+=1;}
            else {
                int j = i; int cn = 0; int cd = 0;
                while(j<size && s[j] != '/') {cn *= 10; cn += s[j]-'0'; j+=1;} j+=1;
                while(j<size && s[j] != '+' && s[j] != '-') {cd *= 10; cd += s[j]-'0'; j+=1;}
                i = j;
                int nd = d*(cd/__gcd(d, cd));
                if(sign) n = (nd/d)*n - (nd/cd)*cn;
                else n = (nd/d)*n + (nd/cd)*cn;
                if(n==0) d=1; else {int g = __gcd(abs(n), nd); n/=g; d = nd/g;}
            }
        }
        return to_string(n)+"/"+to_string(d);
    }
};
```
```c++
class Solution {
public:
    int v(char a)
    {
        return a-'0';
    }
    string fractionAddition(string s) {
        vector<int> n;
        vector<int> d;
        for(int i=0;i<s.size();i++)
        {
            if(s[i]=='/')
            {
                if(i-2>=0 && s[i-2]!='1')
                {
                    if(s[i-2]=='-')
                    {
                        n.push_back(-1*v(s[i-1]));
                    }
                    else
                    {
                        n.push_back(v(s[i-1]));
                    }
                }
                else if(i-2>=0 && s[i-2]=='1' )
                {
                    if(i-3>=0 && s[i-3]=='-')
                     n.push_back(-10);
                    else
                        n.push_back(10);
                }
                else
                {
                    n.push_back(v(s[i-1]));
                }
                if(i+2<s.size() && s[i+2]!='0')
                {
                    d.push_back(v(s[i+1]));
                }
                else if(i+2<s.size() && s[i+2]=='0')
                {
                    d.push_back(10);
                }
                else
                {
                    d.push_back(v(s[i+1]));
                }
            }
        }
        int x=1;
        for(int i: d)
        {
            if(x%i!=0)
            {
                x=x*i;
            }
        }
        int tt=0;
        for(int i=0;i<d.size();i++)
        {
            int y=x/d[i];
            tt=tt+(y*n[i]);
        }
        int l;
        l= __gcd(tt,x);
        int t=tt/fabs(l);
        int y=x/fabs(l);
        
        string e;
        string qq=to_string(t); 
         string ww=to_string(y);
        e=qq+'/'+ww;
        return e;
    }
};
```