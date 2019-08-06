---
layout: post
title:  "445. Add Two Numbers II"
date: 2019-08-02 20:37:00 -0400
categories: articles
---
You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Follow up:
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

Example:
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```
```c++

class Solution {
public:
    int numberOfBoomerangs(vector<vector<int>>& points) {
        int dist[1000];
        int totalNumber = 0;
        for(int i=0;i<points.size();i++){
            for(int j = 0;j<points.size();j++){
                int distX = points[i][0]-points[j][0];
                int distY = points[i][1]-points[j][1]; 
                dist[j] = distX*distX + distY*distY;
            }
            sort(dist, dist+points.size());
            int count = 1;
            for(int k = 1;k<points.size();k++){
                if(dist[k] != dist[k-1])
                {
                    totalNumber+=(count*(count-1));
                    count = 1;
                }
                else
                    count++;
            }
            totalNumber+=count*(count-1);
        }
        return totalNumber;
    }
};
```
```c++
class Solution {
public:
    int numberOfBoomerangs(vector<vector<int>>& points) {
        int res = 0;
        for (int i = 0; i < points.size(); i++) {
            unordered_map<int, int> fequencies;
            for (int j = 0; j < points.size(); j++) {
                if (i != j) {
                    fequencies[dis(points[i], points[j])]++;
                }
            }
            for (auto const &p: fequencies) {
                res += (p.second * (p.second - 1));
            }
        }
        

        return res;
    }
private:
    int dis(vector<int> &p1, vector<int> &p2) {
        return pow((p1[0] - p2[0]), 2) + pow((p1[1] - p2[1]), 2);
    }
};
```
```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<ListNode*> l1_stk;
        stack<ListNode*> l2_stk;
        
        ListNode* l1_head = l1;
        ListNode* l2_head = l2;
        
        while(l1!=NULL) {
            l1_stk.push(l1);
            l1 = l1->next;
        }
        
        while(l2!=NULL) {
            l2_stk.push(l2);
            l2 = l2->next;
        }
        
        bool carry = false;
        while(!l1_stk.empty() && !l2_stk.empty()) {
            int temp=0;
            if (carry) 
                temp = l1_stk.top()->val+l2_stk.top()->val+1;
            else
                temp = l1_stk.top()->val+l2_stk.top()->val;
            
            if (temp >= 10) {
                temp = temp-10;
                carry = true;
            } else {
                carry = false;
            }
            l1_stk.top()->val = temp;
            l2_stk.top()->val = temp;
            l1_stk.pop();
            l2_stk.pop();
        }
        
        if (carry) {
            if (l1_stk.empty() && l2_stk.empty()) {
                ListNode* newNode = new ListNode(1);
                newNode->next = l1_head;
                return newNode;
            } else if (!l1_stk.empty()) {
                while(!l1_stk.empty() && carry) {
                    int temp = l1_stk.top()->val+1;
                    if (temp>=10) {
                        temp = temp-10;
                    } else {
                        carry = false;
                    }
                    l1_stk.top()->val = temp;
                    l1_stk.pop();
                }
                
                if (carry) {
                    ListNode* newNode = new ListNode(1);
                    newNode->next = l1_head;
                    return newNode;   
                } else 
                    return l1_head;
                
            } else {
                cout << "Should be here" << endl;
                while(!l2_stk.empty() && carry) {
                    int temp = l2_stk.top()->val+1;
                    if (temp>=10) {
                        temp = temp-10;
                    } else {
                        carry = false;
                    }
                    l2_stk.top()->val = temp;
                    l2_stk.pop();
                }
                
                if (carry) {
                    ListNode* newNode = new ListNode(1);
                    newNode->next = l2_head;
                    return newNode;   
                } else 
                    return l2_head;
            }
        } else {
            
            if (l1_stk.empty())
                return l2_head;
            else 
                return l1_head;    
        }
        
    }
};
```
```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int n1 = getLength(l1), n2 = getLength(l2);
        int carry = 0;
        ListNode* head = new ListNode(1);
        head->next = n1 > n2 ? addElm(l1, l2, n1 - n2, carry) : addElm(l2, l1, n2 - n1, carry);
        return carry == 1 ? head : head->next;
    }
private:
    int getLength(ListNode* l) {
        int len = 0;
        while (l != nullptr) {
            l = l->next;
            len++;
        }
        return len;
    }
    
    ListNode* addElm(ListNode* l1, ListNode* l2, int k, int& carry) {
        if (l2 == nullptr) return nullptr;
        ListNode* p = new ListNode(l1->val);
        if (k > 0) {
            p->next = addElm(l1->next, l2, k - 1, carry);
        } else {
            p->val += l2->val;
            p->next = addElm(l1->next, l2->next, k, carry);
        }
        p->val += carry;
        carry = p->val / 10;
        p->val = p->val % 10;
        return p;
    }
};
```