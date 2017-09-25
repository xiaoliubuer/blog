---
layout: post
title:  "Design Tiny URL(System Design)"
date:   2017-09-06 15:24:11 -0400
categories: system_design
---

#### __Question 1:__ Why We Need a Tiny URL?
As we know that in out normal social media such as Facebook and Twitter. We want to share a article or a video to our post. However, there is words limitation in our post, in facebook, the limit is 147 words. Right? Thus, if we want to share this link to our post, then it has occupy lots of seats of our words.

http://www.espn.com/nba/story/_/id/20595342/nba-cleveland-cavaliers-guard-isaiah-thomas-faces-uncertain-return-potentially-career-ending-hip-injury

Thus, how to shorten the URL? 

As we all know that each URL or we can say URIs must unique in the web. It is unique link to one specific web page.

Thus if there is anohter service which can help to transfer the long URL to short one. That will be a super good idea.


To analysis a system, we have structured method to use which is called SNAKE representing 
* Scenario
* Necessary
* Application
* Kilobit
* Evolve


#### __Question 2:__ How many services should TinyURL provide?
So, normally, we only a long URL, now we want to get a short or tiny one. Thus, first service is, how to get a tiny URL form a long URL. 

OK, suppose we got a tiny from a long one, but 


Decision 


#### Necessary:

1 Million user daily active user.

What kind of actions they will take?
What they will do every?

Create has new long URL then want to transfer to TinyURL.

Write:
	0.1% * 1 million * 10(new URL) = 100,000

	RPS: 100,000 / 86400s  = 1.2 

Read: 
	100% * 1 million * 5  = 5,000,000
	RPS: 5,000,000 / 86400 = 60

But here, peak hour 
	peak hour probobly is 5 time of normal time.

Thus, write will be 1.2 * 5 = 6 rps in peak hour
Read will be 300 rps.

One server can handle 1000 rps is OK.



That will be 1Million write request and 1Million Tiny and Long pair in database.

Write: 
Read:

Each action will bring different volume requests to tolerance the system.


#### Application:

 short_url = to_short( long_url );

 long_url  = to_long( short_url);

It will be like a key value pair.


Collision:

How to deal with collision.
Time
Not short 
How to create a short URL to 

How many URL can support

2^32

SHA1, MD5

URL clean: Free this map.

Every machine can support 


In database: Insert into database, one row represent one mapping.

1: url1
2: url2
3: url3

The shortURL

10 N
0~9 + A~Z + a~z
10 + 26 + 26

If 1 year 1Million every year.

0  ...  9  10  ... 35 36 ... 61 

0  ...  9   a  ... z  A  ... z

62*62*62*62*62*62

62bit

From one machine, then to multipal machine. 

Distributed system

#### __Kilobit__

Average size of longURL = 100 byte.
1 character is 1 type. Suppose a URL has 100 characters, then it will use 100 bytes.

Average size of id = 4 bytes(int)

Status = 4 bytes


Daily new URL = 100,000 * 108 = 10.8MB

Yearly URL = 10.8 MB * 365 ~ 4GB


Base62 do not support URL clean


Service 

#### __Evolve__

How to implement time-limited service?

Set a expiration period(in status)
	* delete url in 3 years after its creation
	* delete url if it is not used during the past 3 years

	- Base10 or Base62 Methods can't reuse expired short_url

How to cache? Improve read or write?

 	- Pre-load: Load all data to memory.

Memcached(https//memcached.org)

If one machine can't support the service, then we have to use distributed system.


#### __CAP__
First of all, it is issue in distributed system, not a sigle machine.

Three promises:

Consistency: every read receives the most recent write or an error.(等同于所有节点访问同一份最新的数据副本）



就是假设我从一个node写，一个node读同时进行，那么我读到的一定是我最新写进去的，而不是旧一点的数据。

Availability: Every request receives a response, without guarantee that it contains the most revent version of the information（每次请求都能获取到非错的响应——但是不保证获取的数据为最新数据）

Partition Tolerance: The system continues to operate despite an arbitrary number of messages being dropped by the network between nodes.（以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择)


Distributed systems

Distributed system features:
No distributed system is safe from network failure.

A


			P


C

CA: RDBMS 
CP: mongodb
AP: cassreada


High read
Low  write


Evolve - Replication

Load Balancer

server 		server 		server 		server		server

write-write conflict
write use master slave

Read-write	conflict
AP

large a amount of data to store, high demand for QPS

Sharding

Coordinator 


server1  server2  server 3

every server are the same
but serverA save 1....1000, serverB save 1001...2000, serverC 2001...3000

single data point

Response 

Response from CHINA
Global coverage
distribute web server
distribute db server


bro






 	






