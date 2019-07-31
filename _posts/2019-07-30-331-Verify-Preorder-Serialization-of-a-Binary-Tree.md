---
layout: post
title:  "331. Verify Preorder Serialization of a Binary Tree"
date: 2019-07-30 19:51:00 -0400
categories: articles
---
One way to serialize a binary tree is to use pre-order traversal. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as #.
```
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
```
For example, the above binary tree can be serialized to the string "9,3,4,#,#,1,#,#,2,#,6,#,#", where # represents a null node.

Given a string of comma separated values, verify whether it is a correct preorder traversal serialization of a binary tree. Find an algorithm without reconstructing the tree.

Each comma separated value in the string must be either an integer or a character '#' representing null pointer.

You may assume that the input format is always valid, for example it could never contain two consecutive commas such as "1,,3".

Example 1:
```
Input: "9,3,4,#,#,1,#,#,2,#,6,#,#"
Output: true
```
Example 2:
```
Input: "1,#"
Output: false
```
Example 3:
```
Input: "9,#,#,1"
Output: false
```
```c++
class Solution {
public:
    bool isValidSerialization(string preorder) {
        vector<string> vs;
        int bp=0, ep, ns;
        ns = preorder.size();
        stack<string> stk;
        //stk.push();
        while(bp < ns) {
            ep = preorder.find_first_of(",", bp);
            if (ep == string:: npos) ep = ns;
            vs.push_back(preorder.substr(bp, ep-bp));
            bp = ep+1;
        }
        ns = vs.size();
        for (bp =0; bp<ns-1 ; bp ++) {
            if (vs[bp] != "#") stk.push(vs[bp]);
            else if (stk.empty()) return false;
            else stk.pop();
        }
        return stk.empty() && vs[bp] == "#";
    }
};
```
```c++
class Solution {
  
  bool isValidSubtree(const string& preorder, int& bpos) {
    if(bpos >= preorder.size()) {
      return false;
    }
    
    int epos = bpos;
    while(epos != preorder.size() && preorder[epos] != ',') {
      ++epos;
    }
    
    if(preorder[bpos] == '#') {
      bpos = epos+1;
      return true;
    } else {
      bpos = epos+1;
      return (isValidSubtree(preorder, bpos) && (isValidSubtree(preorder, bpos)));
    }
    
  }
  
public:
  bool isValidSerialization(string preorder) {
    int bpos = 0;
    return isValidSubtree(preorder, bpos) && ( bpos == preorder.size()+1);
  }
};
```
```c++
class Solution {
public:
bool isValidSerialization(string preorder) {
    int degree = 1;
    string temp;
    stringstream ss;
    ss << preorder;
    while (getline(ss, temp, ',')) {
        degree--;
        if (degree < 0)
            return false;
        if (temp[0] != '#')
            degree += 2;
    }
    return degree == 0;
}
};
```
```c++
class Solution {
public:

vector<string> split(const string &s, char delim) {
    stringstream ss(s);
    string item;
    vector<string> tokens;
    while (getline(ss, item, delim)) {
        tokens.push_back(item);
    }
    return tokens;
}

    bool isValidSerialization(string preorder) {
        if(preorder.size()<=0)
          return true;
          
        vector<string> tk = split(preorder, ',');
        int numc=0, nullc=0;
        for(int i=0; i<tk.size(); i++)
        {
            if(tk[i].compare("#")==0)
                nullc++;
            else
                numc++;
            if(nullc>=numc+1 && i != tk.size()-1)
                return false;
        }
        return nullc==numc+1;
    }
};
```