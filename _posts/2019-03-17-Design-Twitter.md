---
layout: post
title:  "355. Design Twitter"
date: 2019-03-17 18:30:23 -0400
categories: articles
---
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

postTweet(userId, tweetId): Compose a new tweet.
getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
follow(followerId, followeeId): Follower follows a followee.
unfollow(followerId, followeeId): Follower unfollows a followee.
Example:
```
Twitter twitter = new Twitter();

// User 1 posts a new tweet (id = 5).
twitter.postTweet(1, 5);

// User 1's news feed should return a list with 1 tweet id -> [5].
twitter.getNewsFeed(1);

// User 1 follows user 2.
twitter.follow(1, 2);

// User 2 posts a new tweet (id = 6).
twitter.postTweet(2, 6);

// User 1's news feed should return a list with 2 tweet ids -> [6, 5].
// Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.getNewsFeed(1);

// User 1 unfollows user 2.
twitter.unfollow(1, 2);

// User 1's news feed should return a list with 1 tweet id -> [5],
// since user 1 is no longer following user 2.
twitter.getNewsFeed(1);
```

```c++
class Twitter {
private:
// userId, set of users whom user is following
    map<int, set<int> >followMap;  
    map<int, vector<pair<int,int> > >post;
    int cnt;
    
public:
    /** Initialize your data structure here. */
    Twitter() {
        cnt=0;
    }
    
    /** Compose a new tweet. */
    void postTweet(int userId, int tweetId) {
	// push time and tweetId, will use this time while sorting the feed
       post[userId].push_back({cnt, tweetId});
        cnt++;
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    vector<int> getNewsFeed(int userId) {
        int k = 10;
        map<int,int> indMap;
        for(int id: followMap[userId]) {
           // cout << "id " << id << endl;
            indMap[id] = post[id].size();
            indMap[id]--;
        }
		// add own id so that we can fetch feed of user itself
        indMap[userId] = post[userId].size();
        indMap[userId]--;
        
        priority_queue<pair<int,int> >pq;
        vector<int> res;
        
        while(k--) {
            for(int id: followMap[userId]) {
                if(indMap[id] >= 0) {
                    pq.push(post[id][indMap[id]]);
                    indMap[id]--;
                }
            }
            if(indMap[userId] >= 0) {
                    pq.push(post[userId][indMap[userId]]);
                    indMap[userId]--;
            }
            
            if(!pq.empty()) {
                res.push_back(pq.top().second);
                pq.pop();
            }
        }
        
        return res;
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    void follow(int followerId, int followeeId) {
        followMap[followerId].insert(followeeId);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    void unfollow(int followerId, int followeeId) {
        followMap[followerId].erase(followeeId);
    }
};
```