---
layout: post
title:  "604. Design Compressed String Iterator"
date: 2019-08-14 19:28:00 -0400
categories: articles
---	
Design and implement a data structure for a compressed string iterator. It should support the following operations: next and hasNext.

The given compressed string will be in the form of each letter followed by a positive integer representing the number of this letter existing in the original uncompressed string.

next() - if the original string still has uncompressed characters, return the next letter; Otherwise return a white space.
hasNext() - Judge whether there is any letter needs to be uncompressed.

Note:
```
Please remember to RESET your class variables declared in StringIterator, as static/class variables are persisted across multiple test cases. Please see here for more details.
```
Example:
```
StringIterator iterator = new StringIterator("L1e2t1C1o1d1e1");

iterator.next(); // return 'L'
iterator.next(); // return 'e'
iterator.next(); // return 'e'
iterator.next(); // return 't'
iterator.next(); // return 'C'
iterator.next(); // return 'o'
iterator.next(); // return 'd'
iterator.hasNext(); // return true
iterator.next(); // return 'e'
iterator.hasNext(); // return false
iterator.next(); // return ' '
```

```c++
class StringIterator {
public:
    string S;
    int curr;
    char c;
    int counter;
    StringIterator(string compressedString) {
        S = compressedString;
        c = S[0];
        curr = 0;
        counter = 0;
    }
    
    char next() {
        if(counter == 0) {
            if(curr >= S.size()) {c = ' '; return ' ';}
            int index = 0;
            c = S[curr++];
            while ((curr + index) < S.size() && isdigit(S[(curr + index)%S.size()])) index++; 
            counter = stoi(S.substr(curr,index));
            curr+=index;
        }
        counter--;
        return c;
    }
    
    bool hasNext() { return (curr < S.size() || counter > 0);}
};
```
```c++

class StringIterator {
public:
    queue<pair<char, int>> q;
    StringIterator(string compressedString) {
        istringstream is(compressedString);
        int cnt;
        char c;
        while (is >> c >> cnt){
            q.push({c, cnt});
        }
    }
    
    char next() {
        if (hasNext()){
            auto &t = q.front();
            --t.second;
            if (t.second == 0){
                q.pop();
            }
            return t.first;
        }
        return ' ';
    }
    
    bool hasNext() {
        return !q.empty();
    }
};
```
```c++
class StringIterator {
public:
    queue<pair<char, int>> q;
    StringIterator(string compressedString) {
        istringstream is(compressedString);
        int cnt;
        char c = ' ';
        while (is >> c >> cnt){
            q.push({c, cnt});
        }
    }
    
    char next() {
        if (hasNext()){
            auto &t = q.front();
            --t.second;
            if (t.second == 0){
                q.pop();
            }
            return t.first;
        }
        return ' ';
    }
    
    bool hasNext() {
        return !q.empty();
    }
};
```