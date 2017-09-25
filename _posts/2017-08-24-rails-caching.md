---
layout: post
title:  "Rails Caching"
date:   2017-08-24 09:53:11 -0400
categories: articles
---


# 1. Basic Caching

3 types: page, action, and fragment. By default Rails provides fragment caching. 

add actionpack-page_caching to Gemfile, then __bundle install__ my terminal

```
gem actionpack-aciton_caching
```


For example, I want to cache my page at Development mode, I need to open the config/environment/development.rb to change the setting below:
```
config.action_controller.perform_caching = true
```
#### 1.1 Page Caching
Page caching allows the request for a generated page to be fulfilled by the webserver without having to go through the entire rails stack. This webserver is serving a file directly from the filesystem you will need to implement cache expireation.

#### 1.2 Action Caching



#### Fragment Caching

When different parts of the page need to be cached and expired separately you can use Fragment caching.
