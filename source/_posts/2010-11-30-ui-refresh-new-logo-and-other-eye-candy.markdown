---
comments: true
date: 2010-11-30 16:31:35
layout: post
slug: ui-refresh-new-logo-and-other-eye-candy
title: UI Refresh- new logo and other eye candy
wordpress_id: 154
categories:
- Business/Marketing
---

I spent some time (and some $$) prettying up the [Quick Brown Frog](http://www.quickbrownfrog.com) user interface.
I bought a logo from [Logosamurai](http://www.logosamurai.com/).  Not bad for $67.

[![Quick Brown Frog logo](/images/2010/11/alternate-edit.jpg)](/images/2010/11/alternate-edit.jpg)

Then I added some gauges to display WPM and accuracy in real-time. They're part of the [Google Visualization API](http://code.google.com/apis/visualization/documentation/gadgetgallery.html), and they're awesome.  That is, when they work. They don't seem to work in IE8, so I've disabled them for that browser.

And they have some pretty odd resizing behavior: if you don't explicitly specify a width attribute, the gauges will _shrink_ every time you update the gauge value. It took me quite a bit of experimentation to figure that one out, but thankfully it seems to be working now.


[![speed test gauges](/images/2010/11/gauges.png)](/images/2010/11/gauges.png)


You can check out the changes in this [typing speed test](http://www.quickbrownfrog.com/#!speedTest:).
