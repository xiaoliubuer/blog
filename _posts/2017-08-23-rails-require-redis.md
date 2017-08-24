---
layout: post
title:  "Rails require Redis"
date:   2017-08-23 13:45:11 -0400
categories: articles
---



#### 1. __Require Redis in Rails__
Before use redis in rails, we need to do some config in rails project.

- __Start Redis__

	I mentioned in last article, we need to run **Redis** first.

	```
	$ redis-server
	```

- __Install Redis gem in Rails project__

	Add redis gem in rails gemfile, then run ``` bundle install ```, the redis gem will be installed in your rails project.
	```ruby
	gem 'redis'
	```

- Create redis global variable

	Create a file called redis.rb in your config/initializers folder with the followling line:

	```ruby
	#config/initializers/redis.rb

	$redis = Redis.new
	```
	Then, you can use ```$redis``` to opreate redis in your Rails project.


#### 2. __Take a try in console__

- Run ruby console

	```
	$ irb

	>
	```

- Require redis module

	```
	> require('redis')
	 => true 
	```

- Create a Redis instance

	```
	> redis = Redis.new

	 => #<Redis client v3.3.3 for redis://127.0.0.1:6379/0>
	```

- Set a string into Redis, and get it.

	```
	> redis.set('myname', 'Adam Yang')
	 => "OK"

	> redis.get('myname')
	 => "Adam Yang" 
	```

#### 3. __Operation for different Data Type__

- __String__

	```
	> redis.set('myname', 'Adam Yang')
	 => "OK"

	> redis.get('myname')
	 => "Adam Yang" 
	```


- __Hash__

  - Use __hmset__ method

	```
	> redis.hmset('myhash', :fname, 'adam', :lname, 'yang', :age, 18)
	 => "OK" 

	> redis.hgetall('myhash')
	 => {"fname"=>"adam", "lname"=>"yang", "age"=>"18"} 

	> redis.hmget('myhash', 'fname')
	 => ["adam"]

	> redis.type 'myhash'
	 => "hash" 
	```
  
   - Use String's __set__ method to imitate

	 ```
	 >myhash2 = { type: 'fruit', name: 'apple', color: 'red', price:'2'}
	  => {:type=>"fruit", :name=>"apple", :color=>"red", :price=>"2"} 

	 > redis.set 'myhash2', myhash2.to_json
	  => "OK" 

	 > redis.type('myhash2')
	  => "string" 

	 > redis.get('myhash2')
	  => "{\"type\":\"fruit\",\"name\":\"apple\",\"color\":\"red\",\"price\":\"2\"}" 

	 > require('json')
	  => true

	 > JSON.parse(redis.get 'myhash2')
	  => {"type"=>"fruit", "name"=>"apple", "color"=>"red", "price"=>"2"}
	 ```

   - Use __mapped_hmset__ method

     ```
     >redis.mapped_hmset 'myhash3', { type: 'fruit', name: 'apple', color: 'red', price:'2'}
      => "OK" 

     >redis.hgetall('myhash3')
     => {"type"=>"fruit", "name"=>"apple", "color"=>"red", "price"=>"2"}
     
     > redis.type('myhash3')
     => "hash"

     ```

- __List__
	In Redis, list is just a linked list.

	```
	> redis.lpush('mylist',['this','is', 'my', 'first', 'try'])
	 => 5 

	> redis.lpush 'mylist', ['yes', 'it'] 
	 => 8
	```
	To return the nodes from a range, now return index from 0 to 5

	```
	> redis.lrange('mylist',0 ,5)
	 => ["it", "yes", "yes", "try", "first", "my"] 
	```
	To return all nodes

	```
	> redis.lrange('mylist',0, -1)
	 => ["it", "yes", "yes", "try", "first", "my", "is", "this"]
	```

	if you want to insert a node at the __tail__ or other place of the list, you need use __linsert__ method. By using __BEFORE__ or __AFTER__ to specify a povit where to insert the node.

	```
	> redis.linsert('mylist', 'BEFORE', 'it', 'hello')
	 => 9

	> redis.lrange('mylist', 0, -1)
	 => ["hello", "it", "yes", "yes", "try", "first", "my", "is", "this"]

	> redis.linsert('mylist', 'after', 'this', 'time')
	 => 12
	```

- __Sets__

	```
	> redis.sadd 'myset', ['aaa', 'bbb']
 	 => 2 

	> redis.smembers 'myset'
 	 => ["thisismyfirsttry", "bbb", "aaa"]
	```

- __Ordered Sets__

	```
	> redis.zadd("zset", 32.0, "member")
	 => true 

	> redis.zadd("zset", [[32.0, "a"], [64.0, "b"]])
	 => 2 

	> redis.zcard("zset")
	 => 3

	> redis.zrange("zset", 0, -1)
 	=> ["a", "member", "b"]
	```

#### All Redis Methods You May Use and details is [here](http://www.rubydoc.info/github/redis/redis-rb/Redis)
```
                                  redis.flushdb                     redis.lrange                      redis.queue                       redis.subscribe_with_timeout
redis.__id__                      redis.freeze                      redis.lrem                        redis.quit                        redis.subscribed?
redis.__send__                    redis.frozen?                     redis.lset                        redis.randomkey                   redis.sunion
redis._bpop                       redis.get                         redis.ltrim                       redis.remove_instance_variable    redis.sunionstore
redis._eval                       redis.getbit                      redis.mapped_hmget                redis.rename                      redis.sync
redis._scan                       redis.getrange                    redis.mapped_hmset                redis.renamenx                    redis.synchronize
redis.append                      redis.getset                      redis.mapped_mget                 redis.respond_to?                 redis.taint
redis.auth                        redis.hash                        redis.mapped_mset                 redis.restore                     redis.tainted?
redis.bgrewriteaof                redis.hdel                        redis.mapped_msetnx               redis.rpop                        redis.tap
redis.bgsave                      redis.hexists                     redis.method                      redis.rpoplpush                   redis.time
redis.bitcount                    redis.hget                        redis.method_missing              redis.rpush                       redis.to_enum
redis.bitop                       redis.hgetall                     redis.methods                     redis.rpushx                      redis.to_json
redis.bitpos                      redis.hincrby                     redis.mget                        redis.sadd                        redis.to_s
redis.blpop                       redis.hincrbyfloat                redis.migrate                     redis.save                        redis.trust
redis.brpop                       redis.hkeys                       redis.mon_enter                   redis.scan                        redis.try_mon_enter
redis.brpoplpush                  redis.hlen                        redis.mon_exit                    redis.scan_each                   redis.ttl
redis.call                        redis.hmget                       redis.mon_synchronize             redis.scard                       redis.type
redis.class                       redis.hmset                       redis.mon_try_enter               redis.script                      redis.unsubscribe
redis.client                      redis.hscan                       redis.monitor                     redis.sdiff                       redis.untaint
redis.clone                       redis.hscan_each                  redis.move                        redis.sdiffstore                  redis.untrust
redis.close                       redis.hset                        redis.mset                        redis.select                      redis.untrusted?
redis.commit                      redis.hsetnx                      redis.msetnx                      redis.send                        redis.unwatch
redis.config                      redis.hvals                       redis.multi                       redis.sentinel                    redis.watch
redis.connected?                  redis.id                          redis.new_cond                    redis.set                         redis.with_reconnect
redis.dbsize                      redis.incr                        redis.nil?                        redis.setbit                      redis.without_reconnect
redis.debug                       redis.incrby                      redis.object                      redis.setex                       redis.zadd
redis.decr                        redis.incrbyfloat                 redis.object_id                   redis.setnx                       redis.zcard
redis.decrby                      redis.info                        redis.persist                     redis.setrange                    redis.zcount
redis.define_singleton_method     redis.inspect                     redis.pexpire                     redis.shutdown                    redis.zincrby
redis.del                         redis.instance_eval               redis.pexpireat                   redis.singleton_class             redis.zinterstore
redis.discard                     redis.instance_exec               redis.pfadd                       redis.singleton_method            redis.zrange
redis.disconnect!                 redis.instance_of?                redis.pfcount                     redis.singleton_methods           redis.zrangebylex
redis.display                     redis.instance_variable_defined?  redis.pfmerge                     redis.sinter                      redis.zrangebyscore
redis.dump                        redis.instance_variable_get       redis.ping                        redis.sinterstore                 redis.zrank
redis.dup                         redis.instance_variable_set       redis.pipelined                   redis.sismember                   redis.zrem
redis.echo                        redis.instance_variables          redis.private_methods             redis.slaveof                     redis.zremrangebyrank
redis.enum_for                    redis.is_a?                       redis.protected_methods           redis.slowlog                     redis.zremrangebyscore
redis.eql?                        redis.itself                      redis.psetex                      redis.smembers                    redis.zrevrange
redis.equal?                      redis.keys                        redis.psubscribe                  redis.smove                       redis.zrevrangebylex
redis.eval                        redis.kind_of?                    redis.psubscribe_with_timeout     redis.sort                        redis.zrevrangebyscore
redis.evalsha                     redis.lastsave                    redis.pttl                        redis.spop                        redis.zrevrank
redis.exec                        redis.lindex                      redis.public_method               redis.srandmember                 redis.zscan
redis.exists                      redis.linsert                     redis.public_methods              redis.srem                        redis.zscan_each
redis.expire                      redis.llen                        redis.public_send                 redis.sscan                       redis.zscore
redis.expireat                    redis.lpop                        redis.publish                     redis.sscan_each                  redis.zunionstore
redis.extend                      redis.lpush                       redis.pubsub                      redis.strlen                      
redis.flushall                    redis.lpushx                      redis.punsubscribe                redis.subscribe 
```


https://github.com/nateware/redis-objects