---
layout: post
title:  "Ruby Error Handling"
date:   2017-09-18 10:24:05 -0400
categories: articles
---

Not my original article:
Source from:
[A Beginner's Guide to Exceptions in Ruby-Starr Horne](http://blog.honeybadger.io/a-beginner-s-guide-to-exceptions-in-ruby/)
[Ruby Exceptions](http://rubylearning.com/satishtalim/ruby_exceptions.html)

# What's an exception?
An exception is a special kind of object, an instance of the class Exception or a descendant of that class that represents some kind of exceptional condition; it indicates that something has gone wrong. When this occurs, an exception is raised (or thrown). By default, Ruby programs terminate when an exception occurs. But it is possible to declare exception handlers. An exception handler is a block of code that is executed if an exception occurs during the execution of some other block of code. Raising an exception means stopping normal execution of the program and transferring the flow-of-control to the exception handling code where you either deal with the problem that's been encountered or exit the program completely. Which of these happens - dealing with it or aborting the program - depends on whether you have provided a rescue clause (rescue is a fundamental part of the Ruby language). If you haven't provided such a clause, the program terminates; if you have, control flows to the rescue clause.

Exceptions are Ruby's way of dealing with unexpected events.

If you've ever made a typo in your code, causing your program to crash with a message like SyntaxError or NoMethodError, then you've seen exceptions in action.

When you raise an exception in Ruby, the world stops and your program starts to shut down. If nothing stops the process, your program will eventually exit with an error message.

Here's an example. In the code below, we try to divide by zero. This is impossible, so Ruby raises an exception called ZeroDivisionError. The program quits and prints an error message.


```ruby 
#exception.rb

	result = 1/0
	puts result
	puts "Hello World!"

```

When run this code, the printed result will be:

```
exception.rb:2:in `/': divided by 0 (ZeroDivisionError)
	from exception.rb:2:in `<main>'
```

And the process crash or shut down directly. Sometimes we don't want or we can't let this process shut down, so we need a way to deal with the unexcepted thing. And ensure the rest program continue to run.

So, if we change the code like this below:

```ruby
begin
	result = 1/0
	puts result
rescue Exception => e
	puts "The error is: " + e
	puts "Hello World!"
end

```
This result is like this:

```
divided by 0
Hello World!
```
Nice, although there's a error, the program still works to the end.

The exception still happens, but it doesn't cause the program to crash because it was "rescued." Instead of exiting, Ruby runs the code in the rescue block, which prints out a message.

This is nice, but it has one big limitation. It tells us "something went wrong," without letting us know what went wrong.

All of the information about what went wrong is going to be contained in an exception object.


# Exception Objects
Exception objects are normal Ruby objects. They hold all of the data about "what happened" for the exception you just rescued.

To get the exception object, you will use a slightly different rescue syntax.

```ruby
# Rescues all errors, an puts the exception object in `e`
rescue => e

# Rescues only ZeroDivisionError and puts the exception object in `e`
rescue ZeroDivisionError => e
```

In the second example above, ZeroDivisionError is the class of the object in e. All of the "types" of exceptions we've talked about are really just class names.

Exception objects also hold useful debug data. Let's take a look at the exception object for our ZeroDivisionError.

```ruby
begin
  # Any exceptions in here... 
  1/0
rescue ZeroDivisionError => e
  puts "Exception Class: #{ e.class.name }"
  puts "Exception Message: #{ e.message }"
  puts "Exception Backtrace: #{ e.backtrace }"
end

# Outputs:
# Exception Class: ZeroDivisionError
# Exception Message: divided by 0
# Exception Backtrace: ...backtrace as an array...
```
Like most Ruby exceptions, it contains a message and a backtrace along with its class name.




# Raising Your Own Exceptions

How to use raise? The raise method is from the Kernel module. By default, raise creates an exception of the RuntimeError class. To raise an exception of a specific class, you can pass in the class name as an argument to raise. 

Whenever use raise, the process will stop and throw a exception. By default, the exception will be an RuntimeError.



So far we've only talked about rescuing exceptions. You can also trigger your own exceptions. This process is called "raising." You do it by calling the raise method.

When you raise your own exceptions, you get to pick which type of exception to use. You also get to set the error message.

Here's an example:
```ruby
begin
  # raises an ArgumentError with the message "you messed up!"
  raise ArgumentError.new("You messed up!")
rescue ArgumentError => e  
  puts e.message
end

# Outputs: You messed up! 
```

```ruby
def inverse(x)  
  raise ArgumentError, 'Argument is not numeric' unless x.is_a? Numeric  
  1.0 / x  
end  
puts inverse(2)  
puts inverse('not a number')  
```


```ruby
def inverse(x)  
  raise ArgumentError, 'Argument is not numeric, cccccccc' unless x.is_a? Numeric  
  1.0 / x  
end  

begin
	inverse("dad")
rescue Exception => e
	puts e.message
	puts "Hello World!"
end

#Result:
#Argument is not numeric, cccccccc
#Hello World!

```




As you can see, we're creating a new error object (an ArgumentError) with a custom message ("You messed up!") and passing it to the raise method.

This being Ruby, raise can be called in several ways:

```ruby
# This is my favorite because it's so explicit
raise RuntimeError.new("You messed up!")

# ...produces the same result
raise RuntimeError, "You messed up!"

# ...produces the same result. But you can only raise 
# RuntimeErrors this way
raise "You messed up!"
```


# Making Custom Exceptions

Ruby's built-in exceptions are great, but they don't cover every possible use case.

What if you're building a user system and want to raise an exception when the user tries to access an off-limits part of the site? None of Ruby's standard exceptions fit, so your best bet is to create a new kind of exception.

To make a custom exception, just create a new class that inherits from StandardError.

```ruby
class PermissionDeniedError < StandardError

end

raise PermissionDeniedError.new()
```

This is just a normal Ruby class. That means you can add methods and data to it like any other class. Let's add an attribute called "action":


```ruby
class PermissionDeniedError < StandardError

  attr_reader :action

  def initialize(message, action)
    # Call the parent's constructor to set the message
    super(message)

    # Store the action in an instance variable
    @action = action
  end

end

# Then, when the user tries to delete something they don't
# have permission to delete, you might do something like this:
raise PermissionDeniedError.new("Permission Denied", :delete)


```

# The Class Hierarchy
We just made a custom exception by subclassing StandardError, which itself subclasses Exception.

In fact, if you look at the class hierarchy of any exception in Ruby, you'll find it eventually leads back to Exception. Here, I'll prove it to you. These are most of Ruby's built-in exceptions, displayed hierarchically:

```ruby
Exception
 NoMemoryError
 ScriptError
   LoadError
   NotImplementedError
   SyntaxError
 SignalException
   Interrupt
 StandardError
   ArgumentError
   IOError
     EOFError
   IndexError
   LocalJumpError
   NameError
     NoMethodError
   RangeError
     FloatDomainError
   RegexpError
   RuntimeError
   SecurityError
   SystemCallError
   SystemStackError
   ThreadError
   TypeError
   ZeroDivisionError
 SystemExit
```
You don't have to memorize all of these, of course. I'm showing them to you because this idea of hierarchy is very important for a specific reason.

Rescuing errors of a specific class also rescues errors of its child classes.

Let's try that again...

When you rescue StandardError, you not only rescue exceptions with class StandardError but those of its children as well. If you look at the chart, you'll see that's a lot: ArgumentError, IOError, etc.

If you were to rescue Exception, you would rescue every single exception, which would be a very bad idea

# Rescuing All Exceptions (the bad way)
If you want to get yelled at, go to stack overflow and post some code that looks like this:

```ruby
# Don't do this 
begin
  do_something()
rescue Exception => e
  ...
end
```
The code above will rescue every exception. Don't do it! It'll break your program in weird ways.

That's because Ruby uses exceptions for things other than errors. It also uses them to handle messages from the operating system called "Signals." If you've ever pressed "ctrl-c" to exit a program, you've used a signal. By suppressing all exceptions, you also suppress those signals.

There are also some kinds of exceptions — like syntax errors — that really should cause your program to crash. If you suppress them, you'll never know when you make typos or other mistakes.

# Rescuing All Errors (The Right Way)
Go back and look at the class hierarchy chart and you'll see that all of the errors you'd want to rescue are children of StandardError

That means that if you want to rescue "all errors" you should rescue StandardError.

```ruby
begin
  do_something()
rescue StandardError => e
  # Only your app's exceptions are swallowed. Things like SyntaxErrror are left alone. 
end
```
In face, if you don't specify an exception class, Ruby assumes you mean StandardError

```ruby
begin
  do_something()
rescue => e
  # This is the same as rescuing StandardError
end
```

# Rescuing Specific Errors (The Best Approach)
Now that you know how to rescue all errors, you should know that it's usually a bad idea, a code smell, considered harmful, etc.

They're usually a sign that you were too lazy to figure out which specific exceptions needed to be rescued. And they will almost always come back to haunt you.

So take the time and do it right. Rescue specific exceptions.

```ruby
begin
  do_something()
rescue Errno::ETIMEDOUT => e
 # This will only rescue Errno::ETIMEDOUT exceptions
end
```
You can even rescue multiple kinds of exceptions in the same rescue block, so no excuses. :)
```ruby
begin
  do_something()
rescue Errno::ETIMEDOUT, Errno::ECONNREFUSED => e
end
```


# First begin rescue

```ruby
begin
	#Code that could create errors
rescue
	# Handles the error
	puts "Can't find the file you provided"
end
```

To specifcy types of error

```ruby
begin
	# Code that could create errors
rescue ArgumentError
	# Handles the error
rescue NoMethodError
	# handle it
rescue 
	# everyhting else
end 

```

What if certain type of error but you don't know the name of the error.

```ruby
begin
	# Code that could create errors
rescue ArgumentError
	# Handles the error
rescue NoMethodError
	# handle it
rescue 
	# everyhting else

ensure
	Happens no matter what!!
end 

```


















