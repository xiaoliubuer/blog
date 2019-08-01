---
layout: post
title:  "384. Shuffle an Array"
date: 2019-07-31 21:00:00 -0400
categories: articles
---
Shuffle a set of numbers without duplicates.

Example:
```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```
```c++
class Solution {
private:
    vector<int> numbers;
public:
    Solution(vector<int>& nums) {
        numbers=nums;
    }
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return numbers;
    }
    
    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        vector<int> result(numbers.begin(),numbers.end());
        for(int i=0;i<result.size();++i){
            int pos=rand()%(result.size()-i);
            swap(result[i+pos],result[i]);
        }
        return result;
    }
};
```
```c++

class Solution {
private:
    vector<int> original;
    vector<int> now;
public:
    Solution(vector<int>& nums) : original(nums), now(nums){
        
    }
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return original;
    }
    
    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        for (int i = 0; i < now.size(); ++i) {
            if (rand() % now.size() != 0) {
                swap(now[i], now[0]);
            }
        }
        return now;
    }
};
```
```c++
class Solution {
public:
    Solution(vector<int>& nums) {
        original = nums;
        arr = nums;
    }

    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return original;
    }

    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        for (int i = 0; i < arr.size(); ++i) {
            swap(arr[i], arr[rand() % arr.size()]);
        }
        return arr;
    }
private:
    vector<int> arr;
    vector<int> original;
};
```
```c++
class Solution {
    vector<int> nums;
public:
    Solution(vector<int> nums) {
        this->nums = nums;
    }
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return nums;
    }
    
    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        vector<int> result(nums);
        for (int i = 0;i < result.size();i++) {
            int pos = rand()%(result.size()-i);
            swap(result[i+pos], result[i]);
        }
        return result;
    }
};
```
```c++
class Solution {
public:
    Solution(vector<int> nums):m_nums(nums) {
    }
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {
        return m_nums;
    }
    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {
        vector<int> res(m_nums);
        int end = res.size();
        for (int i = 0; i < m_nums.size(); ++i){
            int val = rand() % end;
            swap(res[end - 1], res[val]);
            end--; 
        }
        return res;
    }

private:
	vector<int> m_nums;
};
```