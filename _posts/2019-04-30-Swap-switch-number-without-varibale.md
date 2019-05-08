---
layout: post
title:  "10001. Swap switch number without varibale"
date: 2019-04-30 18:51:00 -0400
categories: articles
---

Swap number without variable.

```c++
int main() {
    std::cout << "Hello, World!" << std::endl;
    int a = 10;
    int b = 20;
    cout << "a: " << a << "   b: " << b << endl;
    a ^= b;
    b ^= a;
    a ^= b;
    cout << "a: " << a << "   b: " << b << endl;
    return 0;
}
```