---
layout: post
title:  "635. Design Log Storage System"
date: 2019-07-27 19:31:00 -0400
categories: articles
---
You are given several logs that each log contains a unique id and timestamp. Timestamp is a string that has the following format: Year:Month:Day:Hour:Minute:Second, for example, 2017:01:01:23:59:59. All domains are zero-padded decimal numbers.

Design a log storage system to implement the following functions:

void Put(int id, string timestamp): Given a log's unique id and timestamp, store the log in your storage system.


int[] Retrieve(String start, String end, String granularity): Return the id of logs whose timestamps are within the range from start to end. Start and end all have the same format as timestamp. However, granularity means the time level for consideration. For example, start = "2017:01:01:23:59:59", end = "2017:01:02:23:59:59", granularity = "Day", it means that we need to find the logs within the range from Jan. 1st 2017 to Jan. 2nd 2017.

Example 1:
```
put(1, "2017:01:01:23:59:59");
put(2, "2017:01:01:22:59:59");
put(3, "2016:01:01:00:00:00");
retrieve("2016:01:01:01:01:01","2017:01:01:23:00:00","Year"); // return [1,2,3], because you need to return all logs within 2016 and 2017.
retrieve("2016:01:01:01:01:01","2017:01:01:23:00:00","Hour"); // return [1,2], because you need to return all logs start from 2016:01:01:01 to 2017:01:01:23, where log 3 is left outside the range.
```
Note:
There will be at most 300 operations of Put or Retrieve.
Year ranges from [2000,2017]. Hour ranges from [00,23].
Output for Retrieve has no order required.

```c++
class LogSystem {
public:
    LogSystem() {
    }
    
    void put(int id, string timestamp) {
        logMap[timestamp] = id;
    }
    
    vector<int> retrieve(string s, string e, string gra) {
        string ss, ee;
        if (gra == "Year") {
            ss = s.substr(0, 4) + ":00:00:00:00:00";
            ee = e.substr(0, 4) + ":12:31:23:59:59";
        } else if (gra == "Month") {
            ss = s.substr(0, 7) + ":00:00:00:00";
            ee = e.substr(0, 7) + ":31:23:59:59";   
        } else if (gra == "Day") {
            ss = s.substr(0, 10) + ":00:00:00";
            ee = e.substr(0, 10) + ":23:59:59";
        } else if (gra == "Hour") {
            ss = s.substr(0, 13) + ":00:00";
            ee = e.substr(0, 13) + ":59:59";
        } else if (gra == "Minute") {
            ss = s.substr(0, 16) + ":00";
            ee = e.substr(0, 16) + ":59";
        } else {
            swap(ss, s);
            swap(ee, e);
        }
        
        auto it_start = logMap.lower_bound(ss);
        auto it_end = logMap.upper_bound(ee);
        vector<int> ids;
        
        while (it_start != it_end) {
            ids.push_back(it_start->second);
            it_start++;
        }
        
        return ids;
    }
    
private:
     map<string, int> logMap;
};
```