---
layout: post
title: "Code titbits no. 1"
date: 2012-09-12 11:44
comments: true
categories: 
---

In no particular order, here are the things I learned this week about:

### Github pages

Learned how to set up the bones of this site and deploy to Github via [Tom Ordonez' fine instructions](http://www.tomordonez.com/blog/2012/06/04/creating-a-github-blog-using-octopress/)
<!-- more -->
### Cucumber

Learned this from [Corey Schires' blog](http://coryschires.com):


- consolidate positive and negative assertions in step definitions:

{% codeblock negative assertions in Cuke lang:gherkin %}
Then /^I should( not)? see the following columns: "([^"]*)"$/ do |negate, columns|
  within('table thead tr') do
    columns.split(', ').each do |column|
      negate ? page.should_not(have_content(column)) : page.should(have_content(column))
    end
  end
end
{% endcodeblock %}

The optional `( not)?` gets passed into the step as `negate` var for conditional assertion goodness.


Also learned (true titbits here):

- to get the convenience methods in Cucumber features, create a file `features/support/factory_girl.rb` with the contents `World(FactoryGirl::Syntax::Methods)`
- to be *fair* to both Rspec and Cucumber, it's good to put factories in their own `factories/..` folder at your application root.  If you do this you also need to put this in a file at `features/support/factories.rb`:

{% codeblock factories-support-fil lang:ruby %}
Dir[Rails.root + "factories/*.rb"].each do |file|
  require file
end
{% endcodeblock %}

### Gimp

I wanted to create a transparent png from a black and white image.  This is pretty easy in gimp:

1. add an alpha layer to the image ( Layer > Transparency > Add Alpha Layer )
2. make sure the image is in RGB mode ( Image > Mode > RGB )
3. change the background to alpha ( Colors > Color to Alpha)

That second bit is important; if the image is greyscale the Color to Alpha option will be greyed out...

### Ruby

I'm working my way through the Ruby section of [Seven Languages in Seven Weeks](http://pragprog.com/book/btlang/seven-languages-in-seven-weeks).  This book has been great fun.  I like how broad a scope it takes:  from the simplest features of the language on Day 1 to fairly sophisticated subjects on Day 3.

Here are my answers for the Day 2 exercises:

####1. Find out how to access files with and without a code block.  What is the benefit of the code block?

{% codeblock  File handling in a block lang:ruby %}
file = File.open("guacamole.txt", 'w+')
file << "Holy guacamole"
file.close

# accessing the file in the blocky mode:

File.open("guacamole.txt", 'w+').each { |line| puts line } 
{% endcodeblock %}

One line versus three, and at the end of the block the file closes automatically. Tidy and elegant.

####2. How to translate a hash into an array?  And can the reverse be done?

A trivial task using Ruby's `to_a` method:

{% codeblock hash-to-array-conversion lang:ruby %}
hash = { :bob => "uncle" }
hash.to_a.flatten.map { |word| word.to_s }
{% endcodeblock %}

And the reverse:

{% codeblock array-to-hash-conversion lang:ruby %}
array = w%(bob is your uncle)
hash = Hash[*array]
{% endcodeblock %}


####3. Can you iterate through a hash?

Yes, thusly:

{% codeblock hash-iteration lang:ruby %}
hash = {:bob => "uncle", :belinda => "aunt"}
hash.each { |key, value| puts "#{key} - #{value}" }

{% endcodeblock %}

####4. Ruby arrays can be used as stacks.  What other data structures do arrays support?

I tried to find a comprehensive list of array data structures but this is all I came up with.  I'll add more when I find em.

{% codeblock array-data-structures lang:ruby %}

# Queues/Dequeues

queue = []
queue.push("delightful").push("queue")
queue.unshift("2").unshift("1")

queue.pop  # => "queue"
queue.shift # => "1"

# Lists

list = %w(a bag of tricks)
list.insert(1, "special")
list.map! { |e| e == "bag" ? "hag" : e }

# Bag/Set

bag = %w(ceci n'est pas une emeraude)
bag2 = %w(non pas du tout une emeraude)
set = bag | bag2 # => ["ceci", "n'est", "pas", "une",
                 #     "emeraude", "non", "du", "tout"]
# Matrices

m = [[1,2,3], [4,5,6], [7,8,9]]
m.transpose # => [[1,4,7], [2,5,8], [3,6,9]]

{% endcodeblock %}


####5.  Print the content of an array of sixteen numbers, four numbers at a time, using just each.  Then do the same with `each_slice` in Enumerable.

I couldn't find an `each` implementation I liked so I came up with this.

{% codeblock array-slice-n-dice lang:ruby %}
a = [*(1..16)]
4.times { p (a.shift(4)) }

# and the same done with `each_slice`:

a.each_slice(4) { |slice| p slice }

{% endcodeblock %}


####6.  Write a simple grep that will print the lines of a file having any occurrences of a phrase anywhere in that line.  Include line numbers if you like.

{% codeblock simple-grep lang:ruby %}

def simple_grep(pattern, filename)
  regex = %r{pattern}
  File.foreach(filename).with_index do |line, line_num|
    p "#{line_num - line}" if regex =~ line 
  end
end
 
{% endcodeblock %} 

