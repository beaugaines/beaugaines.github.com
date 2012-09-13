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

### Cucumber

Learned all this from [Corey Schires' blog](http://coryschires.com):


- consolidate positive and negative assertions:

{% codeblock lang:gherkin %}
Then /^I should( not)? see the following columns: "([^"]*)"$/ do |negate, columns|
  within('table thead tr') do
    columns.split(', ').each do |column|
      negate ? page.should_not(have_content(column)) : page.should(have_content(column))
    end
  end
end
{% endcodeblock %}

The optional `( not)?` gets passed into the step as `negate` var for conditional assertion goodness.


### Gimp

I wanted to create a transparent png from a black and white image.  This is pretty easy in gimp:

1. add an alpha layer to the image ( Layer > Transparency > Add Alpha Layer )
2. make sure the image is in RGB mode ( Image > Mode > RGB )
3. change the background to alpha ( Colors > Color to Alpha)

That second bit is important; if the image is greyscale the Color to Alpha option will be greyed out...

### Ruby

I'm working my way through the Ruby section of [Seven Languages in Seven Weeks](http://pragprog.com/book/btlang/seven-languages-in-seven-weeks).  This book has been great fun.  I like how broad a scope it takes:  from the simplest features of the language on Day 1 to fairly sophisticated subjects on Day 3.

Here are my answers for the Day 2 exercises:

1. Find out how to access files with and without a code block.  What is the benefit of the code block?

{% codeblock  lang:ruby %}
file = File.open("guacamole.txt", 'w+')
file << "Holy guacamole"
file.close

Accessing the file in the blocky mode:

File.open("guacamole.txt", 'w+').each { |line| puts line } 
{% endcodeblock %}

One line versus three, and at the end of the block the file closes automatically. Tidy and elegant.

2. How to translate a hash into an array?  And can the reverse be done?

