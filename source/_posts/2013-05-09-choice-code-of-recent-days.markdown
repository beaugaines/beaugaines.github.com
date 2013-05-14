---
layout: post
title: "Choice code of recent days"
date: 2013-05-09 14:21
comments: true
categories: 
---

#### I've been busy!

Never manage to post here as much as I'd like.  That, and improving the styling of this site, are both high on my to-do list.  But so far they have been usurped by the tasks at hand.

Most of free my time lately has been spent working on the EdX Python/Intro to Comp. Sci class, as well as a _Mongo for Devs_ class offered online by 10gen.  They've both been challenging but fun and I've learned a ton.<!-- more -->  I wanted to post some of nicer bits of code I've come up with and so here they are:

First this hideously lovely regex, from an exercise in [Dive Into Python](http://www.diveintopython.com), but I like regex so much I came up with it on my own:

{% codeblock hideous-regex lang:python %}

hideousRegex = """
                   # don't match beginning of string, number can start anywhere
\(?                # match a possible opening bracket for area code 
(\d{3})            # area code at beginning of string
\)?                # match a possible closing bracket
\D*                # one or more 'non-word' character - e.g. a hyphen, a space
(\d{3})            # trunk of number - 3 digits
\D*                # one or more 'non-word' chars
(\d{4})            # last 4 digits
\D*                # more non-words
(\d*)?             # maybe an extension of one or more digits
$
"""
{% endcodeblock %}

I also got pretty excited about this recursive range functionn I came up with for the _MIT/EdX_ class:

{% codeblock recursively-ranging lang:python  %}
def recurRange(x,y,step,storage=[])
    storage.append(x)
    if y - x == 1:
        return storage
    else:
        return recurRange(x+step,y,step,storage)

{% endcodeblock %}

RECURSION!  I read somewhere recently that for most any application it is vastly less efficient than an iterative loop.  But it's hard not to love the elegance of it.  I'm smitten.

Also had some fun with a blog scraping script I wrote for a project a couple weeks back:

{% codeblock blog-scrapr lang:ruby %}
require 'nokogiri'
require 'open-uri'
require 'sequel'

BLOG_1 = 'http://widgetmasters.blogspot.com'
freq = Hash.new(0)
page = Nokogiri::HTML(open(BLOG_1))

skip_words = %w(i and of to the a in my her with as on is his like for she that
at an it from - am this our out we are he be was but me their up into one
back through not . were then have by down each around just they its which
all it's over or little you there what against what can had so me been and but
don't let feet side trying get do then than if no how has about only we long more
arm left weight will my few past makes straight know head them where towards
take coud him still tell many two one if your here onto 
)

page.css('div.entry-content p').each do |p|
  words = p.content.split
  words.each { |word| word.gsub!(/(?=\s)(\d|\W)/, '') }
  words.each { |word| freq[word.downcase] += 1 }
end

sorted = freq.sort_by{ |k,v| -v }.last(50)

skip_words.each do |word|
  sorted.delete_if { |k,v| useless_words.include?(k) }
end

# put it into sqlite - in memory, using the cool sequel gem

DB = Sequel.sqlite

DB.create_table :words do
  primary_key :id
  String :name
  Integer :count
end

words = DB[:words]

sorted.each { |key,value| words.insert( :name => key, :count => value) }

# run some queries on the DB

words.order(:count).each { |w| puts w[:name] }


{% endcodeblock %}

And that's it for this edition of _Charlie's Choice Code_.

IN OTHER NEWS - I've been busy recently writing a test suite for [Nfoshare.com](http://www.nfoshare.com).  I love writing tests and it's been interesting to work through their code and test out the classes.  I'm nearly done with the unit tests and will do an integration test suite as well.  I haven't done much test writing recently but I hope to do more of it in coming days.  It's a satisfying way to code...