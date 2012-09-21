---
layout: post
title: "Notes on Scala"
date: 2012-09-19 18:55
comments: true
categories: 
---

I'm taking the free course from Coursera.  I fear it might be a bit beyond me...but I'm gonna risk it.  Here are my notes:


####Lecture 1
- pure imperative programming models are limited by the "Von Neumann" bottleneck; in order to scale up, higher-level abstractions will be required
- in a *restricted sense*, functional programming means programming without mutable variables, assignments, loops, and other imperative control structures.  In a *broader* sense, it just means focusing on the functions.  Functions are values that are 'produced, consumed, and composed'.
- every programming language provides:  a) primitive *expressions* representing the simplest elements, b) ways to *combine* expressions, and c) ways to *abstract* expressions, which introduce a name for an expression by which it can be referred to
- in its most basic sesne, functional programming can be looked at as a bit like using a calculator.  An interactive shell lets one write expressions and responds to their value
- in Scala, definitions can have parameters, e.g.

{% codeblock func-def-params lang:scala %}
scala> def square(x: Double) = x * x
square: (Double)Double
{% endcodeblock %}

- function parameters come with their type, which appears after the colon.  If a return type is given, it appears after the parameter list.
