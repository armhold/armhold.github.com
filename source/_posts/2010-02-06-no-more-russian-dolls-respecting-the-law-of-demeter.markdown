---
comments: true
date: 2010-02-06 22:09:00
layout: post
slug: no-more-russian-dolls-respecting-the-law-of-demeter
title: No More Russian Dolls- Respecting the Law of Demeter
wordpress_id: 31
---




[![](http://armhold.files.wordpress.com/2010/02/russiandolls.jpg?w=300)](http://armhold.files.wordpress.com/2010/02/russiandolls.jpg)






My colleagues and I were struggling with some test ugliness- having to construct complicated object graphs so that a particular Class Under Test could get the argument it needed at test time.  Typically, the CUT would have some call in it like:









    
    <span style="font-family:Times;" class="Apple-style-span"><span style="font-size:medium;line-height:normal;white-space:normal;" class="Apple-style-span"><span style="color:#eeeeee;font-family:'andale mono', 'lucida console', monospace;font-size:small;" class="Apple-style-span"><span style="font-size:small;" class="Apple-style-span"><span style="font-size:14px;line-height:19px;white-space:pre-wrap;" class="Apple-style-span">a.getB().getC().getSomeField()</span></span></span></span></span>





So in order to test something that used A, we'd need to also construct instances of B and C, and wire them all together like a stack of Russian dolls.  For many of our tests, the test _scaffolding_ dwarfed the test proper, making it hard to tell what was really being tested.

To address this, many of our tests were using the [Builder Pattern](http://en.wikipedia.org/wiki/Builder_pattern) to build up these graphs.  This helped somewhat, by centralizing the construction of the objects, and it also led to some some re-use of the scaffolding code. But it still didn't address the underlying problem of having to build up crazy amounts of superfluous objects for the test.

Anyone who's read Refactoring, or the Pragmatic Programmer series will recognize that the [Law of Demeter](http://en.wikipedia.org/wiki/Law_of_Demeter) is in play here.  A, B, and C are too tightly coupled. We all know this is "bad", but somehow this never really bothered me much in production code.  In the age of modern IDEs, it's really easy to track down who is calling whom, and to re-work the code when you feel things have gotten out of hand.

The typical remedy for fixing overly tight coupling is to use encapsulation or a delegate to hide the call chain. This never really struck me as much of an improvement, as it piled **yet another interface** on top of the code.  Having spent too much time toiling with J2EE megaliths, I was wary of adding yet another layer.

But then when we realized what this [inappropriate intimacy](http://c2.com/cgi/wiki?InappropriateIntimacy) was doing to our tests, a light bulb went off. Adding a really small interface to class **A** doesn't really bloat the code, and most importantly- it gives the test code a hook where it can **mock out the single call that it really cares about**, ignoring the rest.







    
    <span style="font-family:Times;" class="Apple-style-span"><span style="font-size:medium;line-height:normal;white-space:normal;" class="Apple-style-span"><span style="color:#eeeeee;font-family:'andale mono', 'lucida console', monospace;font-size:small;" class="Apple-style-span"><span style="font-size:small;" class="Apple-style-span"><span style="font-size:14px;line-height:19px;white-space:pre-wrap;" class="Apple-style-span">class A { private B b; public String getSomeField() { return b.getC().getSomeField(); }}</span></span></span></span></span>







Clients of A now just call A.getSomeField().  Test code that needs an A can get a very simple mock:












    
    <span style="font-family:Times;" class="Apple-style-span"><span style="font-size:medium;line-height:normal;white-space:normal;" class="Apple-style-span"><span style="color:#eeeeee;font-family:'andale mono', 'lucida console', monospace;font-size:small;" class="Apple-style-span"><span style="font-size:small;" class="Apple-style-span"><span style="font-size:14px;line-height:19px;white-space:pre-wrap;" class="Apple-style-span">A mockedA = new A() { @Override public String getSomeField() { return "mockedResult"; }}</span></span></span></span></span>





Yes, class **B**is still sharing too much information here, but unless it's causing a problem, you can leave it alone for now, and apply the same fix to it only if it becomes an issue (maybe YAGNI).


Obviously, this isn't some new industry-changing technique; it's been around forever.  But as a result of applying it judiciously, our test scaffolding has been drastically reduced, and tests are easier to write and understand.


No more Russian dolls.

image credit: [http://en.wikipedia.org/wiki/File:Russian-Matroshka.jpg](http://en.wikipedia.org/wiki/File:Russian-Matroshka.jpg)


![](https://blogger.googleusercontent.com/tracker/3562558747791280858-4474706681478325964?l=garmhold.blogspot.com)
