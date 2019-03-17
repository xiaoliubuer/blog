---
layout: post
title:  "380. Insert Delete GetRandom O(1)"
date: 2019-03-11 20:54:23 -0400
categories: articles
---
Design a data structure that supports all following operations in average O(1) time.

insert(val): Inserts an item val to the set if not already present.
remove(val): Removes an item val from the set if present.
getRandom: Returns a random element from current set of elements. Each element must have the same probability of being returned.
Example:
```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```
# The Answer
```c++
class RandomizedSet {
public:
    /** Initialize your data structure here. */
    RandomizedSet() {
        
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        if (mymap.find(val) == mymap.end()){
            nums.push_back(val);
            mymap[val] = nums.size() - 1;
            return true;
        }
        return false;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    /*
        remove的时候可以从 vector的最后 需要知道那个随机数在 vector里面的第几个位置
    */
    bool remove(int val) {
        if ( mymap.find(val) == mymap.end()) return false;
        else{
            int idx = mymap[val];
            nums[idx] = nums[nums.size() - 1];
            mymap[nums[nums.size() - 1]] = mymap[val]; // So important here!!!
            nums.pop_back();
            mymap.erase(val);
            return true;
        }
    }
    
    /** Get a random element from the set. */
    int getRandom() {
        if (nums.empty()) return 0;
        return nums[rand() % nums.size()];
    }
    vector<int> nums;
    unordered_map<int, int> mymap;
};
```