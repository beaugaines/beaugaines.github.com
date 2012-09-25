---
layout: post
title: "more ruby titbits"
date: 2012-09-25 11:32
comments: true
categories: 
---

I've decided to press ahead and finish _The Well Grounded Rubyist_.  I don't find it easy reading somehow...too much blah blah blah and not enough doing.  But I'll plow through it and then spend a couple days plundering the Ruby APIs.  Some selected titbits from said plowing and plundering:

The topic du jour is _default objects_ and _scope_:

- self inside a singleton method is the object whose singleton method it is.  For ex., a class method can be defined thus:

{% codeblock singleton-class-method lang:ruby %}
class Bob
  def self.x
    puts "This is Bob's class method"
    puts "self: #{self}"
  end
end

# => This is Bob's class method
# => self:  Bob
{% endcodeblock %}

- note that where the receiver of a method's call is self, the receiver can be omitted and will be called implicitly, e.g. `self.babble` is the same as plain 'ole `babble`.  This is most common when one method calls another:

{% codeblock dotless.method_call lang: ruby %}
class Bob
  def is
    puts "This is method 'is'"
  end

  def your_uncle
    puts "Method your_uncle here, about to call is with no dot."
    is
  end
end

bob = Bob.new
bob.your_uncle
# => This is method your_uncle here, about to call is with no dot.
# => This is method 'is'
{% endcodeblock %}

- every instance variable belongs to whatever object is `self` at that current point of execution.
- _scope_ refers to the visibility of _identifiers_ - particularly variables and constants
- note that you can start a new scope without self changing - but sometimes they can change together
- some common global variables:  `$0` - the name of the startup file for the currently running program; `$:` - a list of the directories Ruby searches when you load an external file; `$$` - contains the process id of the current Ruby process
- good practice to avoid global variables as much as possible - to avoid namespace contamination and design things such that data and actions are encapsulated in objects
- _local scope_ is the foundation of every program; at any moment, your program is in a particular local scope. Every top level has its own local scope; every class or module definition has its own local scope; and every call to a method generates a new local scope, with all local vars set to and undefined state
- like a slash at the beginning of a pathname, a `::` in front of a constant means 'start the search for this at the top level'
- class variables are written as `@@bob` and provide a storage mechanism that is shared between a class an instances of that class, but is not visible to any other objects.  This differs from Constants and Globals in that they are visible throughout the program.
- note that class variables are actually _class hierarchy variables_.  Classes and their descendents share the same class variables; so if you set one in a child class, it is shared all the way up the hierarchy