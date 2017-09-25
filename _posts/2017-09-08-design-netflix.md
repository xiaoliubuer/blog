---
layout: post
title:  "Design Netflix(System Design)"
date:   2017-09-08 13:22:11 -0400
categories: system_design
---


What is it? What funcitons it provide?

# __Scenario: case / interface__

1. User management
Register
Login
Profile Display
Profile Management
* multiple profiles, kid restrictions, etc.

2. Movie info

3. Movie Catalog
TV shows, Animation, Dramas, Netflix Originals, etc.
Movie Search
Play operations
play, stop, pause, next, forward,etc.


4. Movie Comments
History Record
Where did you watch last time?


5. Payment
* First-month free use
* Automatic charge
* Payment info management
* Service within User Management

6. Recommendation
* Customized recommendation for users
* Recommend similar movies
* Background service to support Movie Info(list display, search, details)




# __Necessary: Why? constrain / hypothesis__
What is important to a website?
Functional Features
Make it work action!

Non-functional features
Work Well.

Performance:
* __How long__ does it take to respond to a request?
* __Measured__ by delay from sending requests to displaying response
* __Influenced__ by code quality, which database to use, database schema, web framework, etc.


__Reasonable assumption__
difficult to give correct numbers
Need rich experience
reasonable is OK
make it simple in interview

__logical deduction__
* Clear, logical and self-contained 
* A reasonable, practical soluiton 
* Interviewers care how you think and solve the problem.

Assumption
Total subscribers = 100 million
The Number of Movies = 15,000
Daily User Average online time = 1 hour
Recommended for SD quality = 3.0 Mbps

Logical Deduction
KPI  
RPS

SOA 
Service Oriented Articulature 
Services are provided to the other components by application components, through a communicaiton protocol over a network.

A service is discrete unit of functionality that can be accessed remotely and acted upon and updateed independently.


Features:
1. It logically represents a business activity with a specified outcome.
2. It is sel-contained
3. It is a black box for its consumers
4. It may consist of other underlying services.

Advantages
1. Loose Coupling
2. Flexiblity
3. Easier Testing and Debugging
4. Scalability
5. Reusability


Kilobit - User Info
Kilobit, what, how much.

What to store?
* User basic info: username, email, password etc.
* Profile info: kid or not, etc.


How big?
* __User info__
200million users. 1000 byte = 1K
200 million * 1kb  1,000,000k = 1G  20,000,000 = 20G

Which data storage?

Not big, no frequent access or write.
MySQL.


* __Movie info__
what to store
Movie information: name, description, star, etc.
Movie strage url

How big
30,000 * 10kb = 300MB

Which data storage
Not big, frequent access, search by columns
MySQL 

* __Movie Play__
Movie file
Movie play info
30,000 * 40G = 1200T

Very big, frequent access with unique id
Hard Disk Drives Based Server
Movie play info -> MySQL

* __Movie Comments__
What to store:
Who, when, what, for which movie

How big
30,000 * 1,000 * 1kb = 30GB

Which data storage
Frequent access with unique id, no cross search
MongoDB(document NoSQL) JSON file.

* __Payment__
what to store.
Payment info(security): method, balance,etc.
Subscription info: expiration data, discount, etc.

How big
200 million * 100 * 1kb = 20T

* __Recommendation__
What to store 
similar movies for each movie
Recommendation for user
User preferences and history records

How big
200 million * 1 Mb = 200T

Which dta storage
Quick access, weak consistent, big computation
MySQL, Offline S3, Hbase

# __Application: service / algorithm__




# __Kilobit: data__



# __Evolve__








