---
layout: post
title:  "* 269. Alien Dictionary"
date: 2018-12-28 05:57:23 -0400
categories: articles
---
There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of non-empty words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language.

Example 1:
```
Input:
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]

Output: "wertf"
```
Example 2:
```
Input:
[
  "z",
  "x"
]

Output: "zx"
```
Example 3:
```
Input:
[
  "z",
  "x",
  "z"
] 

Output: "" 
```
Explanation: The order is invalid, so return "".
Note:

You may assume all letters are in lowercase.
You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.
If the order is invalid, return an empty string.
There may be multiple valid order of letters, return any one of them is fine.

# 题意
一种新的语言用的是拉丁字母，给了一个list的单词，每个单词已经按照字母排序了，根据这些单词，返回这些字母正确的顺序。
