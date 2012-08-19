---
comments: true
date: 2010-03-08 01:50:00
layout: post
slug: why-no-here-documents-for-java
title: Why no "Here" Documents for Java?
wordpress_id: 38
---

Many languages support the notion of  "[here documents](http://en.wikipedia.org/wiki/Here_document)".  This is a handy way of embedded string literals that would normally have to be escaped right into your program text.  

    
    <span style="color:#eeeeee;" class="Apple-style-span">#!/bin/shcat << "EOF"Here is some text, embedded "right in the middle" of my Bourne shell script.EOF</span>


Having been a standard feature in the Unix shells for decades, this language structure is supported in many newer languages, such as Ruby. Python, PHP and Perl.  But not Java.  In Java, this program would look like:

    
    <span style="color:#eeeeee;" class="Apple-style-span">System.out.println("Here is some text, embedded\n" +" \"right in the middle\"\n" +"of my Java program.\n");</span>


As you can see, the quoting and escaping required to reproduce this text is pretty ugly.


Wouldn't it be nice if we could do this kind of thing in Java too?  It would be great for things like XML and Unit Tests.   How did we get to 2010 without having this as a standard language feature?  Has it ever come up in a JSR?   I haven't had much luck in digging up any information (the term "here document" is remarkably hard to search on.) If you know something about this, please leave a comment.




![](https://blogger.googleusercontent.com/tracker/3562558747791280858-5076794859272087621?l=garmhold.blogspot.com)
