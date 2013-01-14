---
layout: post
title: "more ruby titbits"
date: 2012-09-25 11:32
comments: true
categories: 
---

I've decided to press ahead and finish _The Well Grounded Rubyist_.  I don't find it easy reading somehow...too much blah blah blah and not enough doing.  But I'll plow through it and then spend a couple days plundering the Ruby APIs.  Some selected titbits from said plowing and plundering:
<!-- more -->
#### Default objects and scope:

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

- note that where the receiver of a method's call is self, the receiver can be omitted and will be called implicitly, e.g. `self.babble` is the same as plain old `babble`.  This is most common when one method calls another:

{% codeblock dotless-method-call lang:ruby %}
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
# => Method your_uncle here, about to call is with no dot.
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

#### On private, public, and protected:
- _private_ is a method and takes as an argument a list of the methods you want private; alternately you can define a bunch of instance methods below the call to private
- private means that a method _can't be called with an explicit receiver_ - e.g. `b = Bob.new` would fail b/c you've specified the receiver `b`
- lacking an explicit receiver, private methods adopt the implicit receiver of `self`
- difference between singleton and private methods:  singleton methods belong to only one object, but are not private by default; private methods can be shared by many objects, but may only be called when the object calling them is `self` and has access to said method
- note that the _no explicit receiver_ stipulation is ignored when it comes to _setter_ methods, as these methods require an explicit reciever
- _protected_ methods can be called on a given object x provided that self is an instance of the same class as x - or of an ancestor or descendent of x's class
- what that adds up to is this:  protected is usually used when you want an instance of a class to do something with another instance of the same class
- note also that subclasses inherit method access rules from their superclass, although these can be overriden

#### Writing and using top-level methods
- sometimes you'll want to forego writing classes and modules and just whack together a script; when you do you're coding in the context of the top-level default object - which is _main_
- methods defined at the top level are not instance methods of any class or module but instead are stored as a private instance method of the `Object` class
- for this reason, methods defined at top level can only be called on self - not an an explicit receiver
- many many build in top-level methods - e.g. `print` or `puts` - which are private methods of _Kernel_.  Here is a full list of Ruby's top-level Kernel methods:

{% codeblock top-level-kernel-methods lang:ruby %}
[:Array, :Complex, :Float, :Integer, :Rational, :String, :__callee__, :__method__, :`, :abort, :at_exit, :autoload, :autoload?, :binding, :block_given?, :caller, :catch, :eval, :exec, :exit, :exit!, :fail, :fork, :format, :gem, :gem_original_require, :gets, :global_variables, :initialize_copy, :iterator?, :lambda, :load, :local_variables, :loop, :open, :p, :print, :printf, :proc, :putc, :puts, :raise, :rand, :readline, :readlines, :remove_instance_variable, :require, :require_relative, :select, :set_trace_func, :sleep, :spawn, :sprintf, :srand, :syscall, :system, :test, :throw, :trace_var, :trap, :untrace_var, :warn]
{% endcodeblock %}

#### Control Flow
- you can negate conditions two ways: `if not "bob" == "uncle"` and `if !("bob" == "uncle")`.  Parentheses required in the latter; the _bang_ binds more tightly than the _not_
- if there's any ambiguity at all in your expression, always include parentheses
- if you have an `else` clause, better to use `if` than `unless`
- local variables are initialized to nil at runtime _even if they are not ultimately used_.  This is not the case with instance and class variables
- note that in `case/when` structures, the matches are evaluated using the _case equality_ or 'threequal' operator `===`.  For strings and any object that does not override it it is equivalent to `==` but it can be overridden in any class to define how your objects behave in a case statement
- the `case` statement can be used with no test expression and followed by a sequence of `when` clauses; the first clause whose condition is true will bring home the bacon.  And since the statement evaluates to an object, you can do suchlike:
{% codeblock puts-case-ex lang:ruby %}
puts case
  when meal.breakfast == 'Bacon'
    'Good!'
  when meal.lunch == 'Natto'
    'Bad!'
  else
    'No food better than natto!'
end
{% endcodeblock %}

#### Iteration
- note that `yield` is not the same as returning from a method; yielding takes place while the method is still running.  Also note that the code block is not an argument but rather just part of the method call itself - i.e. part of the syntax
- on blocks and scope:  blocks have access to the variables that already exist when they're created - but variables used in _block parameters_ are *not* the same as variables by the same name in local scope and the latter will not be changed by what happens _en bloqe_.

#### Exceptions
- the beginning of a method definition provides an implicit begin/end context, so you can just put a _rescue_ clause in the body of the method without an explicit _begin_.  However you can get more precise control by including an explicit _begin_; only what lies between _begin_ and _rescue_ will be governed by the clause
- good practice to specify which exception you wish to capture - so you do not mask problems by rescuing excessively
- you can assign the exception message to a variable thusly: `rescue => e`.  And that variable responds to messages; `backtrace` and `message` are especially useful
- note the syntax `raise ArgumentError` looks as if a class is being raised, but in truth an instance of the exception class is being raised
- you can re-raise an exception - for ex. if you want to log the exception but still have it treated as an exception; to do this you just call `raise` from within the rescue block
- you can create your own exception classes by inheriting from `Exception` or one of its descendant classes