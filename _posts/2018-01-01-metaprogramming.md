---
layout: post
title:  "metaprogramming"
date: 2018-01-01 14:17:23 -0400
categories: articles
---

```ruby
class Greeting
  def initialize(text)
    @text = text
  end

  def welcome
    @text
  end
end

byebug
my_object = Greeting.new("Hello")
my_object.class
my_object.class.instance_methods(false)
my_object.methods
my_object.instance_variables
```


```ruby
# compare
my_object.methods
[:welcome, :to_yaml, :instance_of?, :public_send, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :private_methods, :kind_of?, :instance_variables, :tap, :define_singleton_method, :is_a?, :public_method, :extend, :singleton_method, :to_enum, :enum_for, :byebug, :remote_byebug, :debugger, :<=>, :===, :=~, :!~, :eql?, :respond_to?, :freeze, :inspect, :display, :object_id, :send, :to_s, :method, :nil?, :hash, :class, :singleton_class, :clone, :dup, :itself, :taint, :tainted?, :untaint, :untrust, :trust, :untrusted?, :methods, :protected_methods, :frozen?, :public_methods, :singleton_methods, :!, :==, :!=, :__send__, :equal?, :instance_eval, :instance_exec, :__id__]

my_object.class.instance_methods
[:welcome, :to_yaml, :instance_of?, :public_send, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :private_methods, :kind_of?, :instance_variables, :tap, :define_singleton_method, :is_a?, :public_method, :extend, :singleton_method, :to_enum, :enum_for, :byebug, :remote_byebug, :debugger, :<=>, :===, :=~, :!~, :eql?, :respond_to?, :freeze, :inspect, :display, :object_id, :send, :to_s, :method, :nil?, :hash, :class, :singleton_class, :clone, :dup, :itself, :taint, :tainted?, :untaint, :untrust, :trust, :untrusted?, :methods, :protected_methods, :frozen?, :public_methods, :singleton_methods, :!, :==, :!=, :__send__, :equal?, :instance_eval, :instance_exec, :__id__]

```