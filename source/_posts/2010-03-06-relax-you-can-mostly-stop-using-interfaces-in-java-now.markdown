---
comments: true
date: 2010-03-06 22:56:00
layout: post
slug: relax-you-can-mostly-stop-using-interfaces-in-java-now
title: Relax. You can (mostly) stop using Interfaces in Java now.
wordpress_id: 34
---

Detractors of Java are correct to point out a rather unfortunate trend in the Java world- there is a common tendency for developers to turn every non-trivial class into an Interface, and then create an "Impl" that actually does the work.  As a result, your codebase becomes littered with lots of superfluous interfaces that have only a single implementation.

Why do we do this?

Part of it is simple [architecture astronautics](http://www.joelonsoftware.com/articles/fog0000000018.html), but there is actually a very legitimate use case for creating interfaces even when there is only a single implementation- it allows you to mock out your objects so that other classes can be unit tested easily. This is really important in TDD, and so I've come to accept the fact that there will always be some extra "interface noise" in the codebase in order to support a good test infrastructure.  C'est la vie, right?

Along comes JMock 2.5, with its [ClassImposterizer](http://www.jmock.org/mocking-classes.html), backed by cglib.


> _"The _`_ClassImposteriser_`_ creates mock instances without calling the constructor of the mocked class. So classes with constructors that have arguments or call overideable methods of the object can be safely mocked."_




I should point out that this isn't exactly new- JMock 2.5 came out in 2008, so I'm a little late to the game here.  But I haven't seen much written about this.  It's a really huge change.  It means that you can do things like:



``` java
final java.awt.Graphics2D graphics = mockery.mock(java.awt.Graphics2D.class);
mockery.checking(new Expectations() {{ one(graphics).getBackground(); will(returnValue(java.awt.Color.BLUE));}});
```

Of course this is great for testing code that uses libraries that weren't designed with mocking in mind, but even better than that- you can use it against your own code, and save yourself from having to create interfaces.


This means you can go ahead and create your concrete MyComplicatedDAO, without having to create MyComplicatedDAOIFace, MyComplicatedDAOImpl, MyComplicatedDAODummy, etc.  When you need to test code that uses it, you just do:


``` java
final MyComplicatedDAO dao = mockery.mock(MyComplicatedDAO.class);
mockery.checking(new Expectations() {{ one(dao).getLoggedInUsername(); will(returnValue("user1"));}});
```



It's really hard to overstate the significance of this.   It's changed the way I write and test code. It's very refreshing to be able to start out with a concrete class and _write code that does stuff,_ rather than code that merely _talks_ about doing stuff.



Of course there are still very legitimate reasons for using interfaces. If you're designing a library, using interfaces makes life easier on your users. Interfaces are also a great way to decouple code, to remove circular dependencies, and to support callbacks.  All of these are legitimate and appropriate uses, and you shouldn't abandon them.



But use interfaces with reason, and not by reflex.
