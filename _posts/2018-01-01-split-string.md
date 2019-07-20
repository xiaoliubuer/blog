---
layout: post
title:  "Split String c++"
date: 2018-01-01 14:17:23 -0400
categories: articles
---
```c++
// C++ helper to split the string with a delimiter, specify delimiter 
vector<string> split::split_string(string target, char delimiter){
    vector<string> tokens;
    string token;
    istringstream iss(target);
    while( getline(iss, token, delimiter)){
        tokens.push_back(token);
    }
    return tokens;
}
```
```c++
// Good answer with default delimiter ' '
    string str = "this is   a sentence";
    istringstream in(str);

    vector<string> tokens;
    for (string word; in >> word;) {
        tokens.push_back(word);
    }
```

```c++
// By default delimiter is ' ' (space) 
    string str = "this is a sentence";
    istringstream in(str);

    vector<string> tokens;
    for (string word; in >> word;) {
        tokens.push_back(word);
        cout << word << "\n";
    }

    istringstream in2(str);
    string t1;
    while( in2 >> t1){
        tokens.push_back(t1);
        cout << t1 << "\n";
    }
    cout << tokens.size();
```

```c++
// Easy understand
vector<string> split(string s, char delimiter){
   vector<string> tokens;
   string token;
   istringstream tokenStream(s);
   while (getline(tokenStream, token, delimiter)){
      tokens.push_back(token);
   }
   return tokens;
}
```
```c++
// Standard
std::vector<std::string> split(const std::string& s, char delimiter)
{
   std::vector<std::string> tokens;
   std::string token;
   std::istringstream tokenStream(s);
   while (std::getline(tokenStream, token, delimiter))
   {
      tokens.push_back(token);
   }
   return tokens;
}
```
```c++
#include <iostream>
#include <sstream>
#include <vector>

using namespace std;

// for string delimiter
vector<string> split (string s, string delimiter) {
    size_t pos_start = 0, pos_end, delim_len = delimiter.length();
    string token;
    vector<string> res;

    while ((pos_end = s.find (delimiter, pos_start)) != string::npos) {
        token = s.substr (pos_start, pos_end - pos_start);
        pos_start = pos_end + delim_len;
        res.push_back (token);
    }

    res.push_back (s.substr (pos_start));
    return res;
}

int main() {
    string str = "adsf-+qwret-+nvfkbdsj-+orthdfjgh-+dfjrleih";
    string delimiter = "-+";
    vector<string> v = split (str, delimiter);

    for (auto i : v) cout << i << endl;

    return 0;
}
```