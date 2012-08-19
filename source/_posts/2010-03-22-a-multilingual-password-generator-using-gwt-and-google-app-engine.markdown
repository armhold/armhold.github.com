---
comments: true
date: 2010-03-22 04:43:00
layout: post
slug: a-multilingual-password-generator-using-gwt-and-google-app-engine
title: A Multilingual Password Generator using GWT and Google App Engine
wordpress_id: 45
---

My fascination with GWT and Google App Engine continues. This weekend I created a
[multi-lingual memorable password generator](http://create-a-password.appspot.com/).

In order to create memorable passwords I took the [Diceware](http://world.std.com/~reinhold/diceware.html) approach. The idea is simple: create a list of English words, and assign a five digit number to each of them.

    
    23542 click23543 cliff23544 climb23545 clime23546 cling23551 clink


To build up a password, you select some number of these words and combine them (preferably with some further randomization of the case, as well as perhaps inserting some digits or symbols between the words).

To select each word, you roll a die (or fire up a pseudo-random number generator) five times. If you roll 2, 3, 5, 4 and then 3, that corresponds to the word with index 23543. In our case, that's cliff. Keep selecting words in this manner until you've built up a sufficiently long/complex password.

There are a number of password generator sites that seem to be using lots of server-side entropy for password generation, but then blithely download the generated passwords across the Internet to your browser. That doesn't seem like such a good idea. With [create-a-password](http://create-a-password.appspot.com), it all happens right in your browser window, thanks to the magic of GWT.

On the downside, the dictionaries used for word selection are in fact downloaded (and they are well-known anyway). Despite this shortcoming, it still seems safer to me than shipping generated passwords across the network. Using a longer password (Wikipedia recommends [5 or more dice words](http://en.wikipedia.org/wiki/Diceware)) can help with this.

Diceware dictionaries are available in several languages, so it was pretty straightforward to go ahead and use GWT's [i18n](http://code.google.com/webtoolkit/doc/latest/DevGuideI18n.html) to make this program available in a few different languages. Hopefully I haven't butchered the [Spanish](http://create-a-password.appspot.com/Password/Password_es.html) too badly.

I was a little disappointed to learn that GWT doesn't [seem to support automatically recognizing the user's browser locale](http://stackoverflow.com/questions/156412/why-does-gwt-ignore-browser-locale). I expected to be able to automatically detect this, and then offer localized content, but that doesn't seem to be the case. I ended up having to embed a property into each of my localized html pages:

    
    <meta name='gwt:property' content='locale=fr'/>


Another approach is to [use a JSP](http://learngwt.com/articles/mdamour1976/AutomaticGWTInternationalizationdetection.html). Despite this, I think it turned out pretty well.
