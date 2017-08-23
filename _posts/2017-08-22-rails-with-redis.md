---
layout: post
title:  "Rails with Redis"
date:   2017-08-22 10:04:12 -0400
categories: articles
---


# __Rails with Redis__

Redis is a key-value NoSQL database, which has built-in support for data structure like list, sets, and hashes.

Redis has three main peculiarities that sets it apart. 
* Redis holds its database entirely in the memory, using the disk only for persistence.
* Redis has a relatively rich set of data types when compared to many key-value data stores.
* Redis can replicate data to any number of slaves.

#### __1. Download and Inatall__
You can get Redis from [here](https://redis.io/download), just follow the introduction.
* Ubuntu
```
$ wget http://download.redis.io/releases/redis-4.0.1.tar.gz
$ tar xzf redis-4.0.1.tar.gz
$ cd redis-4.0.1
$ make
```
Or you can click the Download icon, download the .gz file. Then extract the file into one of your folder. Then you into the redis folder like below.
```
$ cd redis-4.0.1
```
then run "make" command.
```
$ make
```
The system will run by itself. Finally, you will get a Hint like below:
```
Hint: It's a good idea to run 'make test' ;)
```
Bingo! You've installed successfully!

* Mac
```
brew install redis
```
In my Mac, redis was installed in the path: /usr/local/Cellar/redis/4.0.1 and the config file is /usr/local/etc/redis.conf

#### __2. Run Redis__
* Run Redis server in terminal
```
$ redis-server
```

* Run client
```
$ redis-cli
```

#### __3. First Try__
* Insert a 'key - value' pair into redis
```
$ set key1 'hello world'
OK
```

* Get the value by key
```
$ get key1
"hello world"
```

#### __4. Data Type__

* __String__

```
$ set myname 'adam yang'
OK

$ get myname
"adam yang"
```

* __Hashes__

A Redis hash is a collection of key value pairs. Redis Hashes are maps between string fields and string values. Hence, they are used to represent objects.
Sets the specified fields to their respective values in the hash stored at key. This command overwrites any specified fields already existing in the hash. If key does not exist, a new key holding a hash is created.
```
$ hmset myhash fname adam lname yang age 18 gender man
OK

$ hmget myhash fname
1) "adam"

```

OK, now, if I've add some data in Redis, how can I see __all data__ in my Redis currently? Now, I can use:

```
$ keys *
1) "myhash"
2) "myname"
```
But, what the **TYPE** of these keys? Try to use _TYPE_

```
$ TYPE myhash
hash
```

 Other all [commands](https://redis.io/commands/hmget) about hash. And other all basic [commands](https://redis.io/commands/keys) you will use quite often.

* __Lists__

Redis Lists are simply lists of strings, sorted by insertion order. You can add elements to a Redis List on the head or on the tail.

```
$ lpush mylist 'hello' 'world' 'yes, it is' 'my' 'first' 'try'
(integer) 6

$ lpush mylist 'GMO'
(integer) 7

$ lrange mylist 0 5
1) "GMO"
2) "try"
3) "first"
4) "my"
5) "yes, it is"
6) "world"
```
All other [commands](https://redis.io/commands/lpush) about list.


* __Sets__

Redis Sets are an unordered collection of strings. In Redis, you can add, remove, and test for the existence of members in O(1) time complexity.

```
$ sadd myset this is my
(integer) 3

$ sadd myset first
(integer) 1

$ sadd myset try
(integer) 1

$ sadd myset try
(integer) 0
```
See, every element in set must be unique! We can use __members__ to take a look all my members.

```
$ smembers myset
1) "first"
2) "is"
3) "this"
4) "my"
5) "try"
```

So, now, you may have a question. Set looks quite similar to the List. So, __what's the difference beteween Set and List__. 

In Redis, List represents a collection of string elements sorted according to the order of insertion. SET, on the other hand, is a collection of unique and unsorted string elements. So, the key difference between the two is that SET doesn't allow duplicity and doesn't preserve order. [Read More](https://hashnode.com/post/differences-between-lists-and-sets-in-redis-ciojr30i401g3lh5364dkoqwv)
* __Sorted Sets__

Redis Sorted Sets are similar to Redis Sets, non-repeating collections of Strings. The difference is, every member of a Sorted Set is associated with a score, that is used in order to take the sorted set ordered, from the smallest to the greatest score. While members are unique, the scores may be repeated.

```
$ zadd mysortedset 0 this
(integer) 1

$ zadd mysortedset 0 is
(integer) 1

$ zadd mysortedset 0 my
(integer) 1

$ zadd mysortedset 0 apple
(integer) 1

$ zadd mysortedset 0 abcde
(integer) 1

$ zadd mysortedset 0 abcde
(integer) 0

$ ZRANGEBYSCORE mysortedset 0 100
1) "abcde"
2) "apple"
3) "is"
4) "my"
5) "this"

```




