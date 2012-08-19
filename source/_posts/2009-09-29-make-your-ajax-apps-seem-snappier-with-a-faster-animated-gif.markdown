---
comments: true
date: 2009-09-29 17:58:00
layout: post
slug: make-your-ajax-apps-seem-snappier-with-a-faster-animated-gif
title: Make your Ajax apps seem snappier with a faster animated gif
wordpress_id: 12
categories:
- ajax google-app-engine gwt
---

I often lament the slow speed of the [datastore](http://code.google.com/appengine/docs/java/gettingstarted/usingdatastore.html) used by Google's App Engine, relative to an old-fashioned RDBMS. While Google's datastore is designed to scale well for many, many simultaneous requests, it's a bit pokey on day-to-day CRUD operations.





To deal with this, I implemented a standard "loading" icon that my app displays when accessing the datastore. This way my users will know that something is happening behind the scenes, and not assume that my app has simply stopped responding.




Recently I decided to pretty things up a bit, and so I used the [GIF Generator](http://www.ajaxload.info/) at Ajaxload to create a gif that better matched [my app's](http://recipeminder.appspot.com) color scheme. I installed it, and quickly forgot about it.




Later that same day, I went back to my site and was pleasantly surprised at how quickly App Engine was serving things up- things were noticeably snappier. Initially I chalked it up to the vagaries of the load on App Engine, but then I remembered the updated GIF I had installed. In addition to matching my site's color scheme, it also happened to "spin" faster than the old one. I was shocked at what a huge difference this made! Even though the actual wall-clock time spent is roughly the same, a faster-spinning icon made a huge difference in the _perceived_ wait time. And that's what really matters most, isn't it?




PS: I wanted to include examples of the spinners, but Blogspot does not seem to support animated gifs. :-(







![](https://blogger.googleusercontent.com/tracker/3562558747791280858-1965095450783394696?l=garmhold.blogspot.com)
