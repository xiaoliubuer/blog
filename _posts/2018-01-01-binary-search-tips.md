---
layout: post
title:  "Binary search tips"
date: 2018-01-01 10:52:23 -0400
categories: articles
---

1. 有没有rotate？
2. 有没有相等的？
mid > right, then min at right.
所以，在 rotated binary search， 里面，首先就是拿 mid 和 左边，或者右边比。
根据大小关系，判断取左，还是取右。
