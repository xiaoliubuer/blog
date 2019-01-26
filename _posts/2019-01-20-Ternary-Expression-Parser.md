---
layout: post
title:  "** 439. Ternary Expression Parser"
date: 2019-01-20 15:12:23 -0400
categories: articles
---
Given a string representing arbitrarily nested ternary expressions, calculate the result of the expression. You can always assume that the given expression is valid and only consists of digits 0-9, ?, :, T and F (T and F represent True and False respectively).

Note:

The length of the given string is ≤ 10000.
Each number will contain only one digit.
The conditional expressions group right-to-left (as usual in most languages).
The condition will always be either T or F. That is, the condition will never be a digit.
The result of the expression will always evaluate to either a digit 0-9, T or F.
Example 1:
```
Input: "T?2:3"

Output: "2"

Explanation: If true, then result is 2; otherwise result is 3.
```
Example 2:
```
Input: "F?1:T?4:5"

Output: "4"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(F ? 1 : (T ? 4 : 5))"                   "(F ? 1 : (T ? 4 : 5))"
          -> "(F ? 1 : 4)"                 or       -> "(T ? 4 : 5)"
          -> "4"                                    -> "4"
```
Example 3:
```
Input: "T?T?F:5:3"

Output: "F"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(T ? (T ? F : 5) : 3)"                   "(T ? (T ? F : 5) : 3)"
          -> "(T ? F : 3)"                 or       -> "(T ? F : 5)"
          -> "F"                                    -> "F"
```
# Function signature
```c++
class Solution {
public:
    string parseTernary(string expression) {
        
    }
};
```
# 题意
就是输出嵌套三元表达式的结果。
# 想法
就是扫描，碰到 标记符， ？ 判断 T or F，然后把 ：前 or 后的表达输入到helper function继续判断。

这样类型的题有个大问题就是如果对它的字符串进行分割和处理。
* 在C++里面分割字符串有点麻烦。 同样的 LC394也是同样让人头疼的问题。如何在恰当的时候进行对字符串分局。其实是解题的关键。

这样的题要观察字符串的组成结构。也就是字符串的切割结构。
比如这道题，字符串的结构就是
T ? XXX : XXX
XXX 可以是另一个表达式。

T ? (T ? A : B) : (F ? C : D) 
那么就寻找方便切割的最小单位。

我们最直观的想法就是，pattern match，然后根据pattern，对sub pattern进行类似操作。所以直接就会想到DFS。
但是pattern match是一个不太容易的做法。

所以在C++里，千万不要想着用切割字符串来解题。一定有比这个更好的方便。否则代码量就会很大。不会有这样的题的。45分钟是不可能结出来的。

# 尝试解解
```c++
class Solution {
public:
    string parseTernary(string expression) {
    	int h, q, c;
    	h = 0;
    }
};
```
# 参考答案
```c++
// 不会做说明对这个题的特征还没有分析到位。
// 啥意思？？ 就是到底会形成怎样的结构，这个结构又怎样的共性可以进行解决。
// T ? (T ? A : B) : (F ? C : D) 
class Solution {
public:
    string parseTernary(string expression) {
        int i = expression.size();
        stack<char> st;
        while (i >= 0) {
            if (expression[i] == '?') {
                i--;
                char ch = st.top();
                st.pop();
                if (expression[i] == 'T') {
                    st.pop();
                    st.push(ch);
                }
            } else if (expression[i] != ':') {
                st.push(expression[i]);
            }
            i--;
        }
        return string(1, st.top());
    }
};
```
```c++
// 这道题要好好分析分析。
// T ? (T ? A : B) : (F ? C : D) 
// index = 1 的地方一定是 ？，但是 ？ 后面有两种可能，A，或是 expression。
// 所以如果是A的话，A的后一个是 ：，若果是表达式的话，expression 的下一个是 ？
class Solution {
public:
    string parseTernary(string e) {
        int i = 0;
        return parse(e, i);
    }
private:
// 这道题解的真实太巧妙啦！！
    string parse(string& e, int& i) {
        int idx = i;
        if (i + 1 < e.size() && e[i + 1] == '?') { // recursion case - only if e[i + 1] == '?'
            i += 2;
        	// 如果 ？后面直接是 A 或 B，那么进入下一个parse的时候，a就会返回A，b就会返回B。
        	// 如果 ？后面接的是表达式。
            string a = parse(e, i); // parse both to advance the iterator
            string b = parse(e, ++i);
            return e[idx] == 'T' ? a : b;
        }
        return e.substr(i++, 1); // parse digit or boolean result - just eat a bite
    }
};
```
```c++
// 这个的确更好理解！！卧槽，这是怎么想到的呢？？
// 这个递归用的实在是太妙啦！！
// 如何才能更好的移植呢？移植到其他的题上面。
class Solution {
public:
    string parseTernary(string e) {
        int i = 0;
        return parse(e, i);
    }
private:
    string parse(string& e, int& i) {
        int io = i;
        if( i+1 < e.size() && e[i+1] == '?'){
            i += 2;
            string a = parse(e, i);
            i += 2;
            string b = parse(e, i);
            return e[io] == 'T' ? a : b;
        }
        return e.substr(i,1); 
    }
};
```
```java
// T ? (T ? A : B) : (F ? C : D) 
class Solution {
    public String parseTernary(String expression) {
        if (expression == null || expression.length() == 0) return "";
        int len = expression.length();
        char val = 'T';
        for (int i = 0; i < len; i++) {
            char c = expression.charAt(i);
            if (c == 'T' || c == '?') continue;
            if (c == 'F') {//skip and jump to next valid ':'
                i++;
                if (i >= len || expression.charAt(i) != '?') return "F";
                int count = 1;
                while (i < len && count > 0) {
                    i++;
                    if (expression.charAt(i) == '?') count++;
                    else if (expression.charAt(i) == ':') count--;
                }
            } else if (c == ':') {
                break;
            } else { // c is number
                val = c;
            }
        }
        return String.valueOf(val);
    }
}
```