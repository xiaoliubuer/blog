---
layout: post
title:  "173. Binary Search Tree Iterator"
date: 2019-03-04 20:56:23 -0400
categories: articles
---
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.
Example:
```
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```
Note:

next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.
You may assume that next() call will always be valid, that is, there will be at least a next smallest number in the BST when next() is called.

```c++
// Accepted!!
class BSTIterator {
public:
    BSTIterator(TreeNode* root) {
        helper(root);
        for( auto i : pv){
            my_queue.push(i);
        }
    }
    
    void helper(TreeNode* root){
        if (!root) return;
        // pair<int, TreeNode*> my_paire = make_pair(root->val, root);
        pv.push_back(make_pair(root->val, root));
        helper(root->left);
        helper(root->right);
    }
    /** @return the next smallest number */
    int next() {
        pair<int, TreeNode*> top = my_queue.top();
        my_queue.pop();
        return top.first;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !my_queue.empty();
    }
    // auto compare = [](pair<int, TreeNode*> A, pair<int, TreeNode*> B){ A.first > B.first; }
    struct Compare {
        bool operator()(pair<int, TreeNode*> A, pair<int, TreeNode*> B){ return A.first > B.first; }
    };
    priority_queue<pair<int, TreeNode*>, vector<pair<int, TreeNode*>>, Compare> my_queue;
    vector<pair<int, TreeNode*>> pv;
};
```
```c++
void sort_queue(vector<pair<int, string>> q){

    cout << "Print from PQ: \n";
//    []( adjist a, adjlist b ) { return a.second > b.second; };
    auto mycomp = [](pair<int, string> A, pair<int, string> B){
        return A.first > B.first;
    };
    priority_queue<pair<int, string>, vector<pair<int, string>>, decltype(mycomp) > my_queue(mycomp);

    for(auto i : q){
        my_queue.push(i);
    }
    while(!my_queue.empty()){
        pair<int, string> top = my_queue.top();
        cout << top.first << "  " << top.second << "\n";
        my_queue.pop();
    }
}

int main() {
    std::cout << "Hello, World!" << std::endl;

    vector<pair<int, string>> my_v;

    for (int i = 0; i < 10; ++i) {
        my_v.push_back(make_pair(i,  string(1, 'A' + i)));
    }

    random_shuffle(my_v.begin(), my_v.end());

    for (int j = 0; j < my_v.size(); ++j) {
        cout << my_v[j].first << " " << my_v[j].second << '\n';
    }

    sort_queue(my_v);
    return 0;
}
```