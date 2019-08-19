---
layout: post
title:  "599. Minimum Index Sum of Two Lists"
date: 2019-08-11 18:20:00 -0400
categories: articles
---	
Suppose Andy and Doris want to choose a restaurant for dinner, and they both have a list of favorite restaurants represented by strings.

You need to help them find out their common interest with the least list index sum. If there is a choice tie between answers, output all of them with no order requirement. You could assume there always exists an answer.

Example 1:
```
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
Output: ["Shogun"]
Explanation: The only restaurant they both like is "Shogun".
```
Example 2:
```
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["KFC", "Shogun", "Burger King"]
Output: ["Shogun"]
Explanation: The restaurant they both like and have the least index sum is "Shogun" with index sum 1 (0+1).
```
Note:
```
The length of both lists will be in the range of [1, 1000].
The length of strings in both lists will be in the range of [1, 30].
The index is starting from 0 to the list length minus 1.
No duplicates in both lists.
```
```c++
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        vector<string> res;
        int min_index_sum = INT_MAX;
        if ( list1.size() == 0 || list2.size() == 0 ) return res;
        unordered_map<string, int> my_map;
        for( int i = 0 ; i < list1.size(); i++ ){
        	my_map[list1[i]] = i;
        }

        for( int i = 0; i < list2.size(); i++ ){
        	auto got = my_map.find(list2[i]);
        	if ( got != my_map.end()){ 
        		int sum_of_index = my_map[list2[i]] + i;
        		if ( sum_of_index > min_index_sum){
        			continue;
        		}
        		if ( sum_of_index < min_index_sum ){
                    min_index_sum = sum_of_index;
        			if( !res.empty() )res.pop_back();
        			res.push_back(list2[i]);
        		}
                else{
                    res.push_back(list2[i]);
                }
        	}
        }
        return res;
    }
};
```
```c++

class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
         unordered_map<string, int> m;
         for(int i=0; i<list1.size(); i++){
             m[list1[i]] = i;
         }
        int min=INT_MAX;
        string ans;
        vector<string> res;
        
        for(int i=0; i<list2.size(); i++){
             unordered_map<string, int>::iterator it= m.find(list2[i]);
            if(it!= m.end()){
                if((it->second + i) < min ) res.clear();
                if((it->second + i) <= min ){
                    min=it->second+i;
                    res.push_back(it->first);
                }
            }
        }
    return res;
        
    }
};
```
```c++
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        unordered_map<string, int> map1;
        for (int i = 0; i < list1.size(); i++) map1.emplace(list1[i], i);
        
        int minInd = INT_MAX;
        vector<string> res;
        for (int i = 0; i < list2.size(); i++) {
            if (!map1.count(list2[i])) continue;
            
            int sum = i + map1[list2[i]];
            if (sum < minInd) res.clear();
            
            if (sum <= minInd) {
                res.push_back(list2[i]);
                minInd = sum;
            }
        }
        return res;
    }
};
```
```c++

class Solution {
public:
    vector<string> findRestaurant(vector<string>& v1, vector<string>& v2) {
    unordered_map<string,int> hashset;
    int min = INT_MAX;
    int sum = 0;
    for(int i = 0; i < v1.size(); i++){
        hashset.insert(make_pair(v1[i],i));
    }
    vector<string> v3;
    for(int i = 0; i < v2.size(); i++){
        if(hashset.count(v2[i])){
            sum = hashset[v2[i]] + i;
            if(min > sum){
                min = sum;
                v3 = {v2[i]};
            }
            else if(min == sum){
                v3.push_back(v2[i]);
            }
        }
    }
        return v3;
    }
};
```