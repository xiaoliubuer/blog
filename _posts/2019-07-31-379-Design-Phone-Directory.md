---
layout: post
title:  "379. Design Phone Directory"
date: 2019-07-31 20:50:00 -0400
categories: articles
---
Design a Phone Directory which supports the following operations:

get: Provide a number which is not assigned to anyone.
check: Check if a number is available or not.
release: Recycle or release a number.
Example:
```
// Init a phone directory containing a total of 3 numbers: 0, 1, and 2.
PhoneDirectory directory = new PhoneDirectory(3);

// It can return any available phone number. Here we assume it returns 0.
directory.get();

// Assume it returns 1.
directory.get();

// The number 2 is available, so return true.
directory.check(2);

// It returns 2, the only number that is left.
directory.get();

// The number 2 is no longer available, so return false.
directory.check(2);

// Release number 2 back to the pool.
directory.release(2);

// Number 2 is available again, return true.
directory.check(2);
```
```c++
class PhoneDirectory {
public:
    /** Initialize your data structure here
        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
    PhoneDirectory(int maxNumbers) {
        for (int i = 0; i < maxNumbers; i++)
            s.insert(i);
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    int get() {
        if (s.empty()) return -1;
        int num = *(s.begin());
        s.erase(s.begin());
        return num;
    }
    
    /** Check if a number is available or not. */
    bool check(int number) {
        return s.count(number);
    }
    
    /** Recycle or release a number. */
    void release(int number) {
        s.insert(number);
        return;
    }
    
private:
    unordered_set<int> s;
};
```