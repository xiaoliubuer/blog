---
layout: post
title:  "537. Complex Number Multiplication"
date: 2019-08-09 20:41:00 -0400
categories: articles
---
Given two strings representing two complex numbers.

You need to return a string representing their multiplication. Note i2 = -1 according to the definition.

Example 1:
```
Input: "1+1i", "1+1i"
Output: "0+2i"
Explanation: (1 + i) * (1 + i) = 1 + i2 + 2 * i = 2i, and you need convert it to the form of 0+2i.
```
Example 2:
```
Input: "1+-1i", "1+-1i"
Output: "0+-2i"
Explanation: (1 - i) * (1 - i) = 1 + i2 - 2 * i = -2i, and you need convert it to the form of 0+-2i.
```
Note:
```
The input strings will not have extra blank.
The input strings will be given in the form of a+bi, where the integer a and b will both belong to the range of [-100, 100]. And the output should be also in this form.
```
```c++
class Solution {
public:
    string complexNumberMultiply(string a, string b) {
        string ans = "";
        int ar,ai,br,bi,cr,ci;
        int len_a = a.length();
        int len_b = b.length();
        ar = stoi(strtok(const_cast<char*> (a.c_str()),"+"));
        ai = stoi(strtok(nullptr,"+"));
        br = stoi(strtok(const_cast<char*> (b.c_str()),"+"));
        bi = stoi(strtok(nullptr,"+"));
        cr = ar*br-ai*bi;
        ci = ar*bi+ai*br;
        ans = ans+to_string(cr)+"+"+to_string(ci)+"i";
        return ans;
    }
};
```

```c++
class Solution {
public:
    void tokanize(string s, int* a){
        int idx = 0;
        string token = "";
        for(int i = 0; i < s.size(); i++){
            if(s[i] != '+' && i != s.size() - 1)    token += s[i];
            else{
                a[idx] = stoi(token);
                token = "";
                idx++;
            }
        }
        
    }
    
    string complexNumberMultiply(string aa, string bb) {
        int a[2] = {0};
        int b[2] = {0};
        tokanize(aa, a);
        tokanize(bb, b);

        int c[2] = {0};
        c[0] = a[0] * b[0] - a[1] * b[1];
        c[1] = a[1] * b[0] + b[1] * a[0];
                
        return to_string(c[0]) + "+" + to_string(c[1]) + "i";
    }
};
```