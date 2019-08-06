---
layout: post
title:  "428. Serialize and Deserialize N-ary Tree"
date: 2019-08-02 19:49:00 -0400
categories: articles
---
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize an N-ary tree. An N-ary tree is a rooted tree in which each node has no more than N children. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that an N-ary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following 3-ary tree
as [1 [3[5 6] 2 4]]. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.
Note:

N is in the range of [1, 1000]
Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

```c++
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(Node* root) {
        if (!root) return "";
        stack<Node*> stk;
        if (root) {
            stk.push(root);
        }
        stringstream ss;
        while (!stk.empty()) {
            auto node = stk.top();
            stk.pop();
            if (node) {
                ss << node->val << " ";
                stk.push(nullptr);
                for (auto itr = node->children.rbegin();
                    itr != node->children.rend(); ++itr) {
                    stk.push(*itr);
                }
            } else {
                ss << ") ";
            }
        }
        return ss.str();
    }
    // Decodes your encoded data to tree.
    Node* deserialize(string data) {
        if (data.empty()) return nullptr;
        stringstream ss(data);
        string token;
        stack<Node*> stk;
        Node* root = nullptr;
        while (ss >> token) {
            if (token == ")") {
                stk.pop();
            } else {
                Node* node = new Node(stoi(token));
                if (stk.empty()) {
                    root = node;
                } else {
                    stk.top()->children.push_back(node);
                }
                stk.push(node);
            }
        }
        return root;
    }
};
```

```c++
class Codec {
private:
    void dfs(Node* root, string& res) {
        if (!root || !root->children.size()) return;
        for (Node* node : root->children) {
            res += '(' + to_string(node->val);
            dfs(node, res);
            res += ')';
        }
    }
    
public:
    // Encodes a tree to a single string.
    string serialize(Node* root) {
        string res = "";
        if (!root) return res;
        res += to_string(root->val);
        dfs(root, res);
        return res;
    }

    // Decodes your encoded data to tree.
    Node* deserialize(string data) {
        if (!data.length()) return nullptr;
        int i = 0;
        while (i < data.length() && isdigit(data[i])) ++i;
        Node* root = new Node(stoi(data.substr(0, i)), {});
        int j = i, k = 0;
        while (j < data.length()) {
            if (data[j] == '(') ++k;
            else if (data[j] == ')') --k;
            if (k == 0) {
                Node* child = deserialize(data.substr(i + 1, j - i - 1));
                root->children.push_back(child);
                i = j + 1;
            }
            ++j;
        }
        return root;
    }
};
```
```c++
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(Node* root) {
        if(root == NULL) return "";
        string ret = to_string(root->val);
        for(auto& p : root->children){
            if(p != NULL){
                ret += '(' + serialize(p)  + ')';
            }
        }
        return ret; 
    }

    // Decodes your encoded data to tree.
    Node* deserialize(string data) {
        if(data == "") return NULL;
        int cnt = 0;
        int val = 0;
        int idx = 0;
        while(idx < data.size() && data[idx] >= '0' && data[idx] <= '9'){
            val = 10*val + data[idx]-'0';
            idx++;
        }
        Node* ret = new Node();
        ret->val = val;
        int left = idx;
        while(idx < data.size()){
            if(data[idx] == '('){
                // left = idx;
                cnt++;
            } else if(data[idx] == ')') {
                cnt--;
                if(cnt == 0){
                    string subdata = data.substr(left+1,idx-left-1);
                    ret->children.push_back(deserialize(subdata));
                    left = idx+1;
                }
                
            }
            idx++;
        }
        
        return ret;
    }
};
```