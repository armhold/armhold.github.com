---
layout: post
title: "Thoughts on Ruby/Rails"
date: 2012-08-19 20:58
comments: true
categories: 
---

Java has been my primary programming language since, well, pretty much since it was first released back in 1995 or so.

In May of this year I decided to dive head-first into Ruby. Because you know, Ruby is
[so over](http://news.ycombinator.com/item?id=4383495) [now](http://mashable.com/2011/03/10/node-js).
Here are my observations so far.


![Ruby Books](/images/2012-08-19/ruby-books.jpg "Ruby Books")

## How did we get here?

I've always been a fan of static typing. I love having a compiler tell me my program is syntactically correct,
or warn me about potential problems.

I love that my [IDE](http://www.jetbrains.com/idea) can tell me (instantly, and with 100% certainty)
everywhere a particular class or method is used. I love being able to click through a method call and
keep drilling, all the way down to the JRE library source. That's powerful stuff. So what prompted me to
explore a dynamically typed language like Ruby?

In short, it was Wicket's
[CompoundPropertyModels](http://wicket.apache.org/apidocs/1.5/org/apache/wicket/model/CompoundPropertyModel.html).
CompoundPropertyModels allow you to easily set a common model for a given page, which individual components on the page
can then reference via strings.  They also let you reach deep into nested models (violating the
[Law of Demeter](http://en.wikipedia.org/wiki/Law_of_Demeter)) like so:

``` java
setModel(new CompoundPropertyModel(addressBean));
// ...
new Label("address.state.zipCode"));
```

I had avoided these constructions for quite a while because of the lack of typesafety. But a recent project
with a particularly complicated model hierarchy led me to start using it- it was just so damn convenient. One thing
led to another, and eventually I started questioning static typing altogether.



## Static vs Dynamic Typing


Steve Yegge, [grok](https://plus.google.com/u/0/110981030061712822816/posts/KaSKeg4vQtz), etc.

##  Code Coverage

Code coverage tools in Ruby (e.g. simplecov) tend to report very high coverage rates, but this can
be very misleading. In Ruby, branches tend to be rather condensed, like:

``` ruby
authenticate unless logged_in?
```

This would typically be rendered in Java something as:

``` java
if (! loggedIn) {
   authenticate();
}
```

In Ruby, the test coverage gem will report 100% coverage, even if logged_in never evaluates to true in the test.

## Name Collisions

progressbar vs progress_bar

## Helper Classes

hard to tell where you can call them
hard to override via inheritance
In Rails, Unit/Functional tests are your compiler

