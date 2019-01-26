---
layout: post
title:  "c++ tricky operations"
date: 2018-01-01 10:52:23 -0400
categories: articles
---
1. String

- to_stirng()
```c++
string s = "Hello" + to_stirng(2019);
```
- find()
```c++
// Take any string 
    string s = "dog:cat"; 
  
    // Find position of ':' using find() 
    int pos = s.find(":"); 
  
    // Copy substring after pos 
    string sub = s.substr(pos + 1); 
  
    // prints the result 
    cout << "String is: " << sub; 
  
    return 0; 
```
