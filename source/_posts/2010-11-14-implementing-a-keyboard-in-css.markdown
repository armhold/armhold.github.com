---
comments: true
date: 2010-11-14 13:53:37
layout: post
slug: implementing-a-keyboard-in-css
title: Implementing a keyboard in CSS
wordpress_id: 65
categories:
- GWT
- Technical
---

The predictive keyboard in [QuickBrownFrog](http://www.quickbrownfrog.com/#typing:scottish_terrier.xml:0) has gone through three major iterations so far. At first, I really didn't expect to be able to implement a functional keyboard in GWT (i.e. Javascript+CSS), so I set about drawing the keyboard using Omnigraffle.


### Round 1 - Static Images


[![static keyboard image from Omnigraffle](/images/2010/11/h.png)](/images/2010/11/h.png)

It was fairly easy to whip up the initial design. But then in order to implement key selection I had to create about 100 variations of this with all the different keys selected. That was painful.

GWT provides a way to [bundle image resources](http://code.google.com/webtoolkit/doc/latest/DevGuideUiImageBundles.html), and that helped keep the image size in check, but it was still a bit slow on the client until all the images were cached. Unfortunately this also meant that I had to create a mongo ImageResources.java interface, which listed every foo_key() foo_keyShifted(), etc.

So this clearly wasn't optimal, but it worked for a while. Then when I decided it was time to change the "selected" color, and I had to manually re-color every single PNG. Obviously, this was not going to scale, and it was time to re-think my approach. So I set about experimenting with ways to programmatically generate the keyboard.


### Round 2 - SVG


First up, I went with SVG. The SVG Canvas is not natively supported in every browser, but the [GWT-Graphics library](http://code.google.com/p/gwt-graphics/) gracefully uses VML in Internet Explorer, making it effectively cross-platform. This worked pretty well, and after a day and a half tweaking the layout, I had a functional SVG keyboard. And finally I was able to change the look of the keys (color, shape, etc) very easily with a couple lines of code. I was thrilled!

Unfortunately VML on IE was still kind of slow, and also had some occasional rendering problems. Still, this was far better than a huge set of static images, so I stuck with SVG for a while. When it came time to re-design the website with a new color scheme I was really thrilled to not have to update 100+ keyboard images in Omnigraffle again.!

Round 3 - CSS

With launch-time approaching, the speed issues with VML have really started to become a problem. Clearly, a _predictive_ keyboard needs to be faster than the typist using it, and my keyboard was not keeping up. That led me to my third iteration- CSS.

Since my SVGKeyboard.java was created as an [AbsolutePanel](http://google-web-toolkit.googlecode.com/svn/javadoc/2.1/index.html?overview-summary.html) subclass, it was fairly straightforward to re-implement it in regular old GWT. Instead of an SVG rect, each key is now simply an AbsolutePanel, with [InlineLabels](http://google-web-toolkit.googlecode.com/svn/javadoc/2.1/index.html?overview-summary.html) for keycaps.

Implementing the "selected" coloring is as simple as adding and removing a CSS style that specifies the color and background-color. I was even able to make the keys look rounded with the following bit of CSS magic:

    
    -moz-border-radius: 4px;
     border-radius: 4px;


It's perhaps not as pretty as my original Omnigraffle drawing, but that seems a small sacrifice to make for ease of maintenance, should I decide to tweak the design in the future. Plus there's the performance improvement- typing really flies now!

[![CSS Keyboard](/images/2010/11/css_keyboard.png?w=300)](/images/2010/11/css_keyboard.png)
